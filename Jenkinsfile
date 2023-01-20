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
		stage('Source Code Checkout') {
            		steps {
                		script{
                    			git branch: 'main', url: 'https://github.com/sumit3094/democounter-application.git'
                		}
           		}
        	} //stage 
        	stage('Detect Secrets'){
            		steps {
                		script{
                    			sh 'pwd'
                    			sh 'whispers . | jq'
                		}
            		}
        	}//stage
        	stage('Integration testing'){
            		steps{
                		script{
                   			//sh 'mvn verify -DskipUnitTests'
		     			sh 'mvn -B -DskipTests clean verify'
                		}
            		}
       		}//stage
       		stage('Static code analysis'){
			steps{
				script{
					withSonarQubeEnv('sonarserver') {
					sh 'mvn clean package sonar:sonar'
				}
                       // timeout(time: 1, unit: 'HOURS'){
                       // def qg = waitForQualityGate()
                       //     if (qg.status != 'OK') {
                       //     error "Pipeline aborted due to quality gate failure: ${qg.status}"
                       //     }
                       // }
			//}
            		}
		}	       
       		stage("Quality gate") {
            		steps {
                		waitForQualityGate abortPipeline: true
            		}
       		}//stage
       		stage('UNIT Test'){
            		steps{
                		script{
                   			sh 'mvn test'
                		}
            		}
		} //stage
		stage('Dockerfile Scan'){
			steps{
				script{
				      sh "dockle $registry:$BUILD_NUMBER"
				}
		   	}
	       }	    
		stage('Build'){
			steps{
				script{
					sh "pwd"
					dockerImage = docker.build registry + ":$BUILD_NUMBER"
				}
		    	}
	       }//stage 
		stage('Push Image To Registry'){
			steps{
               			script{
					     docker.withRegistry( '', registryCredential ) { 
					     dockerImage.push()
					     }
               			}
	    		}
		}//stage
		stage('Container Image Scanning'){
			steps{
               			script{
					sh "trivy image --format json --output result.json $registry:$BUILD_NUMBER"
       				}
	   		}
       		}	 
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
			archiveArtifacts artifacts: 'result.json', onlyIfSuccessful: true
        		}
    		}
}//pipeline
