pipeline {
    agent any
    tools {
        maven 'mavenlatest'
        jdk 'javademo'
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
       }
       stage('Quality Gate Status'){
           steps{
              script{
                   waitForQualityGate abortPipeline: false, credentialsId: 'latest-sonar'
                    }
                }
	   }//stage
    }
}    
