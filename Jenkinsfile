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
        stage('Secret Scanning'){
            steps {
                script{
                    sh 'pwd'
                    sh 'whispers . | jq'
                }
            }
        }
       stage('UNIT testing'){
            steps{
               script{
                   sh 'mvn test'
               }
           }
       } 
       stage('Integration testing'){
            steps{
                script{
                   sh 'mvn verify -DskipUnitTests'
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
    }
}    
