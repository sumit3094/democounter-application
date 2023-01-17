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
        stage('Dependency Check'){
            steps {
                script{
                    dependencyCheck additionalArguments: '--scan="." --format JSON', odcInstallation: 'owasp-dependency-tool'
                  //  archiveArtifacts allowEmptyArchive: true, artifacts: '${WORKSPACE}/dependency-check-report.xml', onlyIfSuccessful: true
                }
            }
        }
    }
     post {
        always {
           //dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            archiveArtifacts artifacts: 'dependency-check-report.html', onlyIfSuccessful: true
        }
    }
}    
