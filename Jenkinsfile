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
        stage('Secret Scanning'){
            steps {
                script{
                    sh 'pwd'
                    sh 'whispers . | jq'
                }
            }
        }
        stage('Static code analysis'){
            steps{
                script{
                    //withSonarQubeEnv(credentialsId: 'sonarserver') {
                    //sh 'mvn clean package sonar:sonar'
                    //}
                    withSonarQubeEnv() {
                    sh "${maven}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=test-1"
                    }

                        timeout(time: 1, unit: 'HOURS'){
                        def qg = waitForQualityGate()
                            if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                            }
                        }
                }
            }//steps
        }//stage2
        
        stage('Software Composition Analysis'){
            steps {
                script{
                    dependencyCheck additionalArguments: '--scan="." --format JSON', odcInstallation: 'owasp-dependency-tool'
                  //  archiveArtifacts allowEmptyArchive: true, artifacts: '${WORKSPACE}/dependency-check-report.json', onlyIfSuccessful: true
                }
            }
        }
    }
     post {
        always {
           //dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            archiveArtifacts artifacts: 'dependency-check-report.json', onlyIfSuccessful: true
        }
    }
}    
