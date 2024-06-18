pipeline {
    agent {
        docker {
            image 'maven:3.8.1-jdk-11'
            args '-v /root/.m2:/root/.m2'
        }
    }
    environment {
        DOCKER_CREDENTIALS_ID = '1d44ba3e-a2dd-4361-a63e-d9a93e169753'
        DOCKER_IMAGE = 'curveprop/your-spring-boot-app:latest'
    }
    stages {
        stage('Build') {
            steps {
                script {
                    // Checkout the code from the repository
                    checkout scm

                    // Build the project using Maven
                    sh 'mvn clean package'

                    // List contents of the target directory to ensure JAR file is present
                    sh 'ls -al target/'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Login to Docker Hub
                    withDockerRegistry(credentialsId: "$DOCKER_CREDENTIALS_ID") {
                        // Push Docker image
                        sh 'docker push $DOCKER_IMAGE'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    // Deploy the Docker container
                    sh 'docker run -d -p 8080:8080 $DOCKER_IMAGE'
                }
            }
        }
    }
}
