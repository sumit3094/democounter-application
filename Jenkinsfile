pipeline {
  agent any	
    tools {
        maven 'mavenlatest'
        jdk 'javademo'	
    }
    environment{
        imageName = "democounter"
        registryCredential = 'mydockerhub'
        registry = "sumit3094/jenkins-demo"
        dockerImage = ''    
    }		
    stages {
        stage('Git Checkout') {
            steps {
                script{
                    git branch: 'main', url: 'https://github.com/sumit3094/democounter-application.git'
                }
            }
        }  
       stage('Static code analysis'){
            steps{
                script{
                    withSonarQubeEnv('sonarserver') {
                    sh 'mvn clean package sonar:sonar'
                    }
                        timeout(time: 1, unit: 'HOURS'){
                        def qg = waitForQualityGate()
                            if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                            }
                        }
                }
            }
       }//stage
       stage('Docker build'){
           steps{
                script{
	            sh "pwd"
                    customImage = docker.build imageName + ":$BUILD_NUMBER"
                }
            }
        }//stage
	stage('DockerHub Login') {
        	steps {
        	   sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      		}
    	}
	stage('Push Image'){
	   steps{
                script {
             	    docker.withRegistry([url: "", registryCredential]) {
                    dockerImage.push(customeImage)
          		}
               }
        }
    }
}    
