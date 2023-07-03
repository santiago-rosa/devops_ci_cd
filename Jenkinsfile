pipeline {
    //agent any
    agent { docker { image 'maven:3.9.0-eclipse-temurin-11' } }

    

    stages {
       
        // Define dockerImage at a global scope so it can be accessed across different stages
        def dockerImage

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'cd ./billing && mvn clean install'
            }
        }

        stage('Show path') {
            steps {
                sh 'echo $PATH'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // The Dockerfile is in the "billing/" directory
                    dockerImage = docker.build('my-spring-boot-app:latest', './billing')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'd0f7a557-ed1e-483e-af23-04867f7d435c') {
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}
