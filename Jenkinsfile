pipeline {
    agent any

    environment {
        IMAGE_NAME = "mfblessed078/portfolio-site2"
        IMAGE_TAG = "latest"
        CONTAINER_NAME = "portfolio-container2"
    }

    stages {

        stage('Code Pull') {
            steps {
                checkout scm
            }
        }

        stage('Image Build') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "${DOCKER_PASS}" | docker login -u "${DOCKER_USER}" --password-stdin
                        docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop ${CONTAINER_NAME} || true
                    docker rm -f ${CONTAINER_NAME} 2>/dev/null || true
                    docker pull ${IMAGE_NAME}:${IMAGE_TAG}
                    docker run -d -p 8099:80 --name ${CONTAINER_NAME} ${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }
    }
}
