pipeline{
    agent any
    tools { 
        maven 'mavenlatest' 
        jdk 'javademo' 
    } 
    environment{
        imageName = "democounter"
        registryCredential = 'docker-nexus'
        registry = "demoapp.eastus.cloudapp.azure.com:8085"
        dockerImage = ''    
        
    }
    stages {
        stage('Git Checkout'){
            steps{
                script{
                    git branch: 'main', url: 'https://github.com/sumit3094/democounter-application.git'
                }//script
            }//steps
        }stage1

        stage('UNIT testing'){
            steps{
                script{
                    sh 'mvn test'
                }
           }
        }//stage

	stage('Integration testing'){
            steps{
                script{
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }
  
	stage('Static code analysis'){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonarserver') {
                        sh 'mvn clean package sonar:sonar'
                    }
			timeout(time: 1, unit: 'HOURS'){
			def qg = waitForQualityGate()
				if (qg.status != 'OK') {
			        error "Pipeline aborted due to quality gate failure: $(qg.status)"
				}
			}	
                }
            }
        }
  
        //stage('Quality Gate Status'){
        //   steps{
        //      script{
         //          waitForQualityGate abortPipeline: false, credentialsId: 'demotoken'
         //           }
         //       }
	//}//stage

	stage('Docker build'){
           steps{
                script{
	            sh "pwd"
                    customImage = docker.build imageName + ":$BUILD_NUMBER"
                }
            }
        }//stage
	
	stage("Check Image Vulnerability"){
            steps {
                script{
                sh "trivy -v"
               // sh "trivy image -f json -o results.json nginx:1.18"
               // recordIssues(tools: [trivy(pattern: 'results.json')])
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
     }//stage
        stage{
           steps{
		script{
		sh "kubectl  --kubeconfig config_private apply -f Infra/kubernetes/deployment.yml"
		}
           }
        }

    }//stages
}//pipeline
