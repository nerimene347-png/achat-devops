pipeline {
    agent any
    stages {
        stage('Git') {
            steps { checkout scm }
        }
        stage('Maven build') {
            steps {
                dir('apps/server') {
                    sh 'mvn -B clean package -DskipTests'
                }
            }
        }
        stage('Docker image') {
            steps {
                dir('apps/server') {
                    sh 'docker build -f Dockerfile.server -t narimen123/achat-devops:1.0.0 .'
                }
            }
        }
        stage('Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'U', passwordVariable: 'P')]) {
                    sh 'echo $P | docker login -u $U --password-stdin'
                    sh 'docker push narimen123/achat-devops:1.0.0'
                }
            }
        }
        stage('Docker-compose') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }
}
