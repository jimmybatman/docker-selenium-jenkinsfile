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
                	bat "docker build -t containertest ."
					bat "docker tag  containertest containertest:${BUILD_NUMBER}"
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
					bat "docker push containertest:${BUILD_NUMBER}"
					bat "docker push containertest:latest"
				
			        }
                }
            }
        }        
    }
}