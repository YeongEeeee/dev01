pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github-token-key', url: 'https://github.com/YeongEeeee/dev01.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t finish07sds/dev01:1.0 .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    // ⚠️ 반드시 이 형태로 들어가 있어야 토큰이 안 깨지고 도커허브로 들어갑니다!
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push finish07sds/dev01:1.0'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f dev01-deploy.yaml'
                sh 'kubectl apply -f dev01-svc.yaml'
            }
        }
    }
}
