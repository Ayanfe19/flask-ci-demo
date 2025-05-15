pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/ayanfe19/flask-ci-demo.git'
        DOCKER_IMAGE = 'ayanf3d3v/flask-ci-demo'
        DOCKER_CREDENTIALS = credentials('DOCKER_CREDENTIALS')
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    git branch: 'main', url: "${REPO_URL}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_ID} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh '''
                        echo "$DOCKER_CREDENTIALS_PSW" | docker login -u "$DOCKER_CREDENTIALS_USR" --password-stdin
                        docker push ${DOCKER_IMAGE}:${BUILD_ID}
                        docker push ${DOCKER_IMAGE}:latest
                    '''
                }
            }
        }
    }	

    post {
        always {
            echo 'Pipeline execution completed.'
        }
    }
}