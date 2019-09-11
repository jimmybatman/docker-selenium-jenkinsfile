//variables
def network="jenkins-${BUILD_NUMBER}"
def seleniumHub="selenium-hub-${BUILD_NUMBER}"
def chrome="chrome-${BUILD_NUMBER}"
def firefox="firefox-${BUILD_NUMBER}"
def containertest="conatinertest-${BUILD_NUMBER}"
//def report_output="${WORKSPACE}\\search"
   
pipeline {
    agent any
    stages { 	
      
		
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
                  bat "docker run --rm -e SELENIUM_HUB=${seleniumHub} -e BROWSER=firefox -e MODULE=search-module.xml -v ${WORKSPACE}\\search:/usr/share/tag/test-output --network ${network} jimmybatman/selenium"
                  //archive all the files under 'search' directory
                  archiveArtifacts artifacts: 'search/**', fingerprint: true
               
         }
      }
        
    }
	post{
      always {
         bat "docker rm -vf ${chrome}"
         bat "docker rm -vf ${firefox}"
         bat "docker rm -vf ${seleniumHub}"
         bat "docker network rm ${network}"
      } 
	}	  
}