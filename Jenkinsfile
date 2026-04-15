pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t portfolio-site .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop portfolio-container || true
                docker rm portfolio-container || true
                docker run -d -p 8081:80 --name portfolio-container portfolio-site
                '''
            }
        }
    }
}
