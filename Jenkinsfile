pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
                script{
                    git branch: 'main', url: 'https://github.com/sumit3094/democounter-application.git'
                }
            }
        }
        stage('Scan Source Code'){
            steps {
                script{
                    sh 'pwd'
                    sh 'whispers . | jq'
                }
            }
        }
    } 
}    
