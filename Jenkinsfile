pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build with Maven') {
            steps {
                //sh 'mvn clean install'
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
                    docker.build('my-spring-boot-app:latest', '.')
                }
            }
        }*/

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("my-spring-boot-app:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'a81a5674-1d28-4348-8dae-2af1a2569c30') {
                        docker.image('my-spring-boot-app:latest').push()
                    }
                }
            }
        }
    }
}
