stage('Build Docker Image') {
    steps {
        sh '''
        docker build -t manas2994/frontend:${BUILD_NUMBER} ./src/frontend
        '''
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
            docker push manas2994/frontend:${BUILD_NUMBER}
            docker logout
            '''
        }
    }
}

stage('Deploy to GKE') {
    steps {
        sh '''
        kubectl set image deployment/frontend frontend=manas2994/frontend:${BUILD_NUMBER}
        kubectl rollout status deployment/frontend
        '''
    }
}
