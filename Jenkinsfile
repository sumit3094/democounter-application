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
       }
}

