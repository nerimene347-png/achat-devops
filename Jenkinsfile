pipeline {
    agent any

    stages {
        stage('Git') {
            steps {
                git branch: 'feature-docker', url: 'https://github.com/nerimene347-png/achat-devops.git'
            }
        }

        stage('Building image') {
            steps {
                sh 'docker build -t narimen123/achat-server:1.0.0 -f apps/server/Dockerfile.server apps/server'
            }
        }

        stage('Deploy Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DH_USER', passwordVariable: 'DH_PASS')]) {
                    sh '''
                        echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin
                        docker push narimen123/achat-server:1.0.0
                    '''
                }
            }
        }

        stage('Docker Compose') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }
}
