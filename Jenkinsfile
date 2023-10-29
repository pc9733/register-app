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
               git branch: 'main' , credentialsId: 'git' , url: 'https://github.com/pc9733/register-app'
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
       stage("SonarQube Analysis"){
           steps {
	           script {
		        withSonarQubeEnv(credentialsId: 'jenkins_sonar') { 
                        sh "mvn sonar:sonar"
		        }
	           }	
           }
       }
        stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins_sonar'
                }	
            }

        }
            stage('Build Docker Image') {
              steps{
              script {
                sh "docker build -t register-app:1.0 ."
                sh "docker images"
              }
              }
    }
        

       }
}
