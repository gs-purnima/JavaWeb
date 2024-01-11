pipeline {
    agent {
        node {
            label 'prj3Slv'
        }
    }
    environment {
        registry = "031890266231.dkr.ecr.us-east-1.amazonaws.com/your_ecr_repo"
    }
    stages {
        stage ('SCM') {
            steps {
                git 'https://github.com/gs-purnima/JavaWeb.git'
            }
        }
        // Building Docker images
        stage('Building image') {
          steps{
            script {
              dockerImage = docker.build registry
            }
          }
        }
        // Uploading Docker images into AWS ECR
        stage('Pushing to ECR') {
         steps{  
             script {
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 031890266231.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'docker push 031890266231.dkr.ecr.us-east-1.amazonaws.com/your_ecr_repo:latest'
                    }
                }
            }
         // Stopping Docker containers
         stage('stop previous containers') {
             steps {
                sh 'docker ps -f name=mypythonContainer -q | xargs --no-run-if-empty docker container stop'
                sh 'docker container ls -a -fname=mypythonContainer -q | xargs -r docker container rm'
             }
        }
    }
}

