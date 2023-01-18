pipeline {
  agent {
    docker { image 'node:16-alpine' }	
  }	
    tools {
        maven 'mavenlatest'
        jdk 'javademo'
	docker 'docker-tool'
    }
    environment{
        imageName = "democounter"
        registryCredential = 'docker-nexus'
        registry = "demoapp.eastus.cloudapp.azure.com:8085"
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

    }
}    
