
pipeline {
    agent any

    environment {
        DOCKERHUB_REPO = 'pradipnilkanth/gym-website'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out Gym website source code'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build \
                      -t ${DOCKERHUB_REPO}:${BUILD_NUMBER} \
                      -t ${DOCKERHUB_REPO}:latest .
                '''
            }
        }

        stage('Verify Docker Image') {
            steps {
                sh 'docker images ${DOCKERHUB_REPO}'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login \
                          -u "$DOCKER_USER" --password-stdin

                        docker push ${DOCKERHUB_REPO}:${BUILD_NUMBER}
                        docker push ${DOCKERHUB_REPO}:latest

                        docker logout
                    '''
                }
            }
        }
    }
}

