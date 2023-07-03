// Define dockerImage at a global scope so it can be accessed across different stages
def dockerImage

pipeline {
    //agent any
    agent { docker { image 'maven:3.9.0-eclipse-temurin-11' } }

    stages {
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

        /*stage('Build Docker Image') {
            steps {
                script {
                    // The Dockerfile is in the "billing/" directory
                    dockerImage = docker.build('santiagorrosa/my-spring-boot-app:latest', './billing/target')
                }
            }
        }*/

        stage('Build Docker Image') {
            steps {
                script {
                // Move to the directory where the Dockerfile is
                dir('./billing') {
                    // Now Dockerfile and .jar file are both accessible, pass the relative path to .jar file as build argument
                    dockerImage = docker.build("santiagorrosa/my-spring-boot-app:latest", "--build-arg JAR_FILE=target/billing-0.0.1-SNAPSHOT.jar .")
                }
            }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'd0f7a557-ed1e-483e-af23-04867f7d435c') {
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }
}
