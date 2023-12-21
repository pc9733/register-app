pipeline{
    agent any
    tools{
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
       //stage("SonarQube Analysis"){
       //    steps {
	   //        script {
	   //        withSonarQubeEnv(credentialsId: 'jenkins_sonar') { 
       //                 sh "mvn sonar:sonar"
	   //        }
	   //        }	
       //    }
       //}
       // stage("Quality Gate"){
       //    steps {
       //        script {
       //             waitForQualityGate abortPipeline: false, credentialsId: 'jenkins_sonar'
       //         }	
       //     }
       //
       // }
            stage('Build Docker Image') {
              steps{
              script {
                sh "whoami"
                sh "docker build -t register-app:${BUILD_NUMBER} ."
                sh "docker images"
              }
              }
    }
    stage('Push Docker Image to ECR') {
    steps {
        script {
            // Authenticate Docker to ECR (this step is needed before pushing the image)
            sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 120695692422.dkr.ecr.us-east-1.amazonaws.com"

            // Tag the Docker image with the ECR repository URL
            sh "docker tag register-app:1.0 120695692422.dkr.ecr.us-east-1.amazonaws.com/register-app:1.0"

            // Push the Docker image to ECR
            sh "docker push 120695692422.dkr.ecr.us-east-1.amazonaws.com/register-app:${BUILD_NUMBER}"
        }
    }
}
stage('Deploy to EKS') {
    steps {
        script {
            sh "kubect get all"
            // sh "kubectl config use-context <EKS_cluster_context>"
            // sh "kubectl apply -f deployment.yml" 
        }
    }
}

stage('Check Deployment') {
    steps {
        script {
            sh "kubectl get all"
            // sh "kubectl get deployments"
            // sh "kubectl get services"
        }
    }
}
        

       }
}
