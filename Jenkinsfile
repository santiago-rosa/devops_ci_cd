pipeline {
    agent any

   /* tools {
        // We need Maven and Docker installed in the agent
        maven 'Maven 3.9.3' 
        docker 'Docker'
    }*/

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

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("my-spring-boot-app:latest", ".")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'santiagorrosa') {
                        docker.image("my-spring-boot-app:latest").push()
                    }
                }
            }
        }
    }
}
