pipeline{
    agent any
    tools{
        jdk 'jdk11'
        maven 'Maven3'
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }

         }
        stage("Github Checkout"){
            steps{
               git branch: 'main' , credentialsId: 'github' , url: 'https://github.com/pc9733/register-app'
            }
         
        }
        stage("Build-application"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("Test-application"){
            steps{
                sh "mvn test"
            }
        }
}
}