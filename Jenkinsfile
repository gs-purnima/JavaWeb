pipeline {
    agent {
        node {
            label 'Main'
        }
    }
    environment {
        registry = "474011752432.dkr.ecr.us-east-1.amazonaws.com/java"
    }
    stages {
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
                    sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 474011752432.dkr.ecr.us-east-1.amazonaws.com'
                    sh 'docker push 474011752432.dkr.ecr.us-east-1.amazonaws.com/java:latest'
                    }
                }
            }
         // Stopping Docker containers
         stage('stop previous containers') {
             steps {
                sh 'docker ps -f name=mypJavaContainer -q | xargs --no-run-if-empty docker container stop'
                sh 'docker container ls -a -fname=myJavaContainer -q | xargs -r docker container rm'
             }
        }
    }
}
