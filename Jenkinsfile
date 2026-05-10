pipeline {
    agent any

    tools {
        jdk 'jdk21'
        nodejs 'node16'
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from Git') {
            steps {
                git branch: 'main',
                url: 'https://github.com/geethajakka/deployment-of-youtube.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {

                    withDockerRegistry(credentialsId: 'docker') {

                        sh 'docker build -t youtube-clone:latest .'

                        sh 'docker tag youtube-clone:latest balu2629/balugeetha:latest'

                        sh 'docker push balu2629/balugeetha:latest'
                    }
                }
            }
        }

        stage('Deploy to Container') {
            steps {
                script {

                    sh 'docker rm -f youtube-clone || true'

                    sh 'docker run -d --name youtube-clone -p 3000:3000 balu2629/balugeetha:latest'
                }
            }
        }
    }
}
