pipeline {
    agent any
    tools {
        maven 'mavenlatest'
        jdk 'javademo'
    }//tools
    environment{
    	imageName = "democounter"
        registryCredential = 'mydockerhub'
        registry = "sumit3094/jenkins-demo"
        dockerImage = ''   
	DOCKERHUB_CREDENTIALS= credentials('mydockerhub')
    }	//environment
    stages {
        stage('Git Checkout') {
            steps {
                script{
                    git branch: 'main', url: 'https://github.com/sumit3094/democounter-application.git'
                }
            }
        } //stage 
        stage('Secret Scanning'){
            steps {
                script{
                    sh 'pwd'
                    sh 'whispers . | jq'
                }
            }
        }//stage
        stage('UNIT testing'){
            steps{
                script{
                   sh 'mvn test'
               }
           }
		} //stage
        stage('Integration testing'){
            steps{
                script{
                   sh 'mvn verify -DskipUnitTests'
                }
            }
       }//stage
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
                    //customImage = docker.build imageName + ":$BUILD_NUMBER"
		     dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
       }//stage
       stage('Container Security Diagnosis'){
       	   steps{
               	script{
		      sh "dockle $registry:$BUILD_NUMBER"
       		}
	   }
       }	       
       stage('Push Image'){
       	    steps{
               	script{
		     docker.withRegistry( '', registryCredential ) { 
                     dockerImage.push()
		     }
               	}
	    }
	}//stage
	stage('Software Composition Analysis'){
            steps {
                script{
                    dependencyCheck additionalArguments: '--scan="." --format JSON', odcInstallation: 'owasp-dependency-tool'
		}
	    }
	}//stage	
    }//stages
    		post {
        		always {
           		//dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            		archiveArtifacts artifacts: 'dependency-check-report.json', onlyIfSuccessful: true
        		}
    		}
}//pipeline
