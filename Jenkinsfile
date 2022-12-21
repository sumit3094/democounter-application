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
                		}//script
            		}//steps
        	}stage1 
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

	}//stages
}
