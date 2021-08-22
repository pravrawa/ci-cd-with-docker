pipeline {
    agent any
	
	  tools
    {
       maven "Maven 3.3.3"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/pravrawa/ci-cd-with-docker.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn clean package sonar:sonar'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t samplewebapp:$BUILD_NUMBER .'
               
          }
        }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh 'docker run --name LoginWebApp-tomcat -d -p 8091:8080 samplewebapp:$BUILD_NUMBER'
 
            }
        }
 
    }
}
