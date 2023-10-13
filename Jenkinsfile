pipeline{
    agent{
        label "Jenkins-agent"
    }
    tools{
        jdk 'Java17'
        maven 'maven3'
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