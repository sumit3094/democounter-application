pipeline{
    agent any
    tools { 
        maven 'mavenlatest' 
        jdk 'javademo' 
    } 
    environment{
        
        image = "demoapp.eastus.cloudapp.azure.com:8085/democounter"
        registryCredential = 'docker-nexus'        
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
	            sh "pwd"
                    customImage = docker.build image + ":$BUILD_NUMBER"
	            echo "Image Name is"
                    echo "customImage"
                }
            }
        }
	stage('Push Image'){
	   steps{
                script {
            		docker.withRegistry( 'http://demoapp.eastus.cloudapp.azure.com:8085', registryCredential ) {
            		dockerImage.push(customeImage)
          		}
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
        
