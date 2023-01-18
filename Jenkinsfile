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
        }//stage 
    }
}    
