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
        //stage('Install Scanning Tool'){
        //    steps {
        //        script{
        //            sh 'pip3 install whispers'     
        //        }  
        //    }
       // }   
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
