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
                sh 'docker build -t tycoon2506/loginwebapp:$BUILD_NUMBER .'
            }
        }
		stage('Delete Tomcat Container') {
            steps {
				echo 'Deleting Tomcat Container..'
				sh 'docker stop loginwebapp-tomcat'
				sh 'docker rm loginwebapp-tomcat'
				
            }
        }
		stage('Run Docker container on Jenkins Agent') {
            steps {
                echo 'Running Tomcat Container..'
                sh 'docker run --name LoginWebApp-tomcat -d -p 8091:8080 tycoon2506/loginwebapp:$BUILD_NUMBER'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                
            }
        }
    }
}
