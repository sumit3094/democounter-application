pipeline{
    agent any
    tools { 
        maven 'mavenlatest' 
        jdk 'javademo' 
    } 
    stages {
        stage('Git Checkout'){
            steps{
                script{
                    git branch: 'main', url: 'https://github.com/sumit3094/democounter-application.git'
                }
            }
        }
	stage('Docker build'){
           steps{
                script{
                    def customImage = docker.build("my-image:${env.BUILD_ID}")
                }
            }
        }
     }
}
//        stage('UNIT testing'){
//            steps{
//                script{
//                    sh 'mvn test'
//                }
//           }
//        }
//        stage('Integration testing'){
//            steps{
//                script{
//                    sh 'mvn verify -DskipUnitTests'
//                }
//            }
//        }
//        stage('Maven build'){
//            steps{
//                script{
//                    sh 'mvn clean install'
//                }
//            }
//        }
//        stage('Static code analysis'){
//            steps{
//                script{
//                    withSonarQubeEnv(credentialsId: 'demotoken') {
//                        sh 'mvn clean package sonar:sonar'
//                    }
//                   }
//                }
//            }
 //           stage('Quality Gate Status'){
//                steps{
//                    script{
//                        waitForQualityGate abortPipeline: false, credentialsId: 'demotoken'
//                    }
//                }
//            }
        
