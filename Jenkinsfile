pipeline {
  agent any	
    tools {
        maven 'mavenlatest'
        jdk 'javademo'	
    }
    environment{
        imageName = "democounter"
        registryCredential = 'docker-nexus'
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

    }
}    
