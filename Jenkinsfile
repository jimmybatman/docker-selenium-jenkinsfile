pipeline {
    agent any
    stages { 	
        stage('Build Jar') {
            steps {
                bat 'mvn clean package  -DskipTests'
            }
        }
        stage('Build Image') {
            steps {
                script {
                	bat "docker build -t jimmybatman/selenium ."
					bat "docker tag  jimmybatman/selenium jimmybatman/selenium:${BUILD_NUMBER}"
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
				
			        // docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
			        	//app.push("${BUILD_NUMBER}")
			            //app.push("latest")
					bat "docker login -u jimmybatman --password wahjimmy196 docker.io"
					bat "docker push jimmybatman/selenium:${BUILD_NUMBER}"
					bat "docker push jimmybatman/selenium:latest"
				
			        //}
                }
            }
        }
		
      stage('Setting Up Selenium Grid') {
         steps{        
            bat "docker network create ${network}"
            bat "docker run -d -p 4444:4444 --name ${seleniumHub} --network ${network} selenium/hub"
            bat "docker run -d -e HUB_PORT_4444_TCP_ADDR=${seleniumHub} -e HUB_PORT_4444_TCP_PORT=4444 --network ${network} --name ${chrome} selenium/node-chrome"
            bat "docker run -d -e HUB_PORT_4444_TCP_ADDR=${seleniumHub} -e HUB_PORT_4444_TCP_PORT=4444 --network ${network} --name ${firefox} selenium/node-firefox"
         }
      }
      stage('Run Test') {
         steps{
           
                  // a directory 'search' is created for container test-output
                  bat "docker run --rm -e SELENIUM_HUB=${seleniumHub} -e BROWSER=firefox -e MODULE=search-module.xml -v ${WORKSPACE}/search:/usr/share/tag/test-output --network ${network} jimmybatman/selenium"
                  //archive all the files under 'search' directory
                  archiveArtifacts artifacts: 'search/**', fingerprint: true
               
         }
      }
        
    }
	post{
      always {
         sh "docker rm -vf ${chrome}"
         sh "docker rm -vf ${firefox}"
         sh "docker rm -vf ${seleniumHub}"
         sh "docker network rm ${network}"
      }   
}