pipeline {
  agent any	
    tools {
        maven 'mavenlatest'
        jdk 'javademo'	
    }
    environment{
        imageName = "democounter"
        registryCredential = 'mydockerhub'
        registry = "sumit3094/jenkins-demo"
        dockerImage = ''   
	DOCKERHUB_CREDENTIALS= credentials('mydockerhub')
    }		
    stages {
        stage('Git Checkout') {
            steps {
                script{
                    git branch: 'main', url: 'https://github.com/sumit3094/democounter-application.git'
                }
            }
        }  
       
	stage('Docker build'){
           steps{
                script{
	            sh "pwd"
                    customImage = docker.build imageName + ":$BUILD_NUMBER"
                }
            }
        }//stage
	stage('DockerHub Login') {
        	steps {
        	   sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
		   echo 'Login Completed' 
      		}
    	}
	stage('Push Image'){
		steps{
                	script {
             	    	//docker.withRegistry([url: "", registryCredential]) {}
		    	withDockerRegistry([ credentialsId: "mydockerhub", url: "" ])
                    	dockerImage.push(customeImage)
			}
            	}
	}
    }
}    
