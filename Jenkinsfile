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
                	bat 'docker build -t jimmy/containertest .'
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
			         docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
			        	//app.push("${BUILD_NUMBER}")
			            //app.push("latest")
					//bat 'docker login -u "$USERNAME" -p "$PASSWORD" $Harbor_Registry'
					bat 'docker push jimmy/containertest:${BUILD_NUMBER}'
					bat 'docker push jimmy/containertest:latest'
				
			        }
                }
            }
        }        
    }
}