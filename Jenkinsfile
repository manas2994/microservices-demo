pipeline {
    agent any

    stages {

        stage('Verify Tools') {
            steps {
                sh 'pwd'
                sh 'ls -la'
                sh 'git --version'
                sh 'docker --version'
                sh 'kubectl version --client'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t manas2994/frontend:v1 ./src/frontend'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push manas2994/frontend:v1
                        docker logout
                    '''
                }
            }
        }

        stage('Deploy to GKE') {
            steps {
                sh '''
                    kubectl set image deployment/frontend frontend=manas2994/frontend:v1
                    kubectl rollout status deployment/frontend
                '''
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
