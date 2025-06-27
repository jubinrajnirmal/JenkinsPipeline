pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jubinraj/jubin-jenkins:latest'
    }

    triggers {
        githubPush()  
    }

    stages {
        stage('Jubin - Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}")
                }
                echo "Stage: Build Docker Image completed"
            }
        }

        stage('Jubin - Login to Dockerhub') {
            steps {
                withCredentials([string(credentialsId: 'jubinraj', variable: 'dockerhubpwd')]) {
                    sh "docker login -u jubinraj -p ${dockerhubpwd}"
                }
                echo "Stage: Login to Dockerhub completed"
            }
        }

        stage('Jubin - Push image to Dockerhub') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}").push()
                }
                echo "Stage: Push image to Dockerhub completed"
            }
        }
    }

    post {
      always {
        echo 'Pipeline completed.'
      }
      success {
        echo 'Docker image pushed successfully'
      }
      failure {
        echo 'Pipeline failed.'
      }
    }
}
