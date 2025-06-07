pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id') // Jenkins stored credentials
        DOCKERHUB_USERNAME = 'heisenbergzz'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/HeisenbergZS/react-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def branch = env.BRANCH_NAME
                    def imageTag = "${DOCKERHUB_USERNAME}/dev:latest"
                    if (branch == 'master') {
                        imageTag = "${DOCKERHUB_USERNAME}/prod:latest"
                    }
                    sh "docker build -t ${imageTag} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    def branch = env.BRANCH_NAME
                    def imageTag = "${DOCKERHUB_USERNAME}/dev:latest"
                    if (branch == 'master') {
                        imageTag = "${DOCKERHUB_USERNAME}/prod:latest"
                    }
                    sh """
                    echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                    docker push ${imageTag}
                    """
                }
            }
        }

        stage('Deploy') {
            steps {
                // Optionally run deploy.sh or trigger deployment pipeline
                sh "./deploy.sh"
            }
        }
    }
}
