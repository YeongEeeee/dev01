pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                // 1. 깃허브 저장소에서 최신 소스코드를 긁어옵니다.
                git branch: 'main', credentialsId: 'github-token-key', url: 'https://github.com/YeongEeeee/dev01.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // 2. 로컬에 있는 Dockerfile을 기반으로 이미지를 빌드합니다.
                // 임시로 태그는 1.0으로 지정합니다. (본인 도커허브 ID가 있다면 finish07sds 대신 넣으셔도 됩니다)
                sh 'docker build -t finish07sds/dev01:1.0 .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                // 3. 젠킨스 웹에 저장한 'dockerhub-creds' 열쇠를 꺼내와서 도커허브에 로그인하고 이미지를 푸시합니다.
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh 'docker push finish07sds/dev01:1.0'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                // 4. 쿠버네티스(K3s) 클러스터에 배포 설정 파일들을 적용합니다.
                // (이 파일들은 다음 단계에서 터미널에 만들 예정입니다!)
                sh 'kubectl apply -f dev01-deploy.yaml'
                sh 'kubectl apply -f dev01-svc.yaml'
            }
        }
    }
}
