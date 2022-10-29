pipeline {
    agent any
	tools {
        maven 'Maven 3.3.3'
        
    }
    stages {
		stage('Clean Up Workspace') {
            steps {
                echo 'Deleteing old workspace..'
				cleanWs()
            }
        }
		stage('SCM Checkout') {
            steps {
                echo 'Check out the code from Github..'
				git url: 'https://github.com/pravrawa/ci-cd-with-docker.git'
				
            }
        }
        stage('Build & SonarQube analysis') {
            steps {
                echo 'Building & Code Analysis..'
				withSonarQubeEnv('SonarQube') {
                sh 'mvn clean package sonar:sonar'
				}
            }
        }
		stage('Quality Gate') {
            steps {
                echo 'Waiting for Quality Gate results..'
				//timeout(time: 1, unit: 'MINUTES') {
                //waitForQualityGate abortPipeline: false
                //}
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image..'
                sh 'docker build -t nexus-demo:8085/tycoon2506/loginwebapp:$BUILD_NUMBER .'
            }
        }
	    stage('Push image to Nexus Repository ') {
            steps {
                 echo 'Uploading Docker Image to Nexus repository..'
				withDockerRegistry(credentialsId: 'nexus-cred', url: 'http://nexus-demo:8085') {
				sh  'docker push nexus-demo:8085/tycoon2506/loginwebapp:$BUILD_NUMBER'
				}
			}
		}	
		stage('Delete Tomcat Container') {
            steps {
				echo 'Deleting Tomcat Container..'
				sh 'docker stop LoginWebApp-tomcat'
				sh 'docker rm LoginWebApp-tomcat'
				
            }
        }
		stage('Run Docker container on Jenkins Agent') {
            steps {
                echo 'Running Tomcat Container..'
                sh 'docker run --name LoginWebApp-tomcat -d -p 8091:8080 nexus-demo:8085/tycoon2506/loginwebapp:$BUILD_NUMBER'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                
            }
        }
    }
}
