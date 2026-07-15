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
    }
}
