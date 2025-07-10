pipeline {
    agent any

    environment {
        IMAGE_NAME = "react-app"
        CONTAINER_NAME = "react-app-container"
        PORT = "80"
        REACT_APP_API_BASE = credentials('REACT_APP_API_BASE')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Print Environment Variables') {
            steps {
                echo "🔧 환경 변수 설정:"
                echo "REACT_APP_API_BASE: $REACT_APP_API_BASE"
            }
        }

        stage('Create .env File') {
            steps {
                sh '''
                cat > .env << EOF
                REACT_APP_API_BASE=$REACT_APP_API_BASE
                EOF
                echo "✅ .env 파일 생성 완료:"
                cat .env
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME ."
            }
        }

        stage('Deploy') {
            steps {
                sh "docker rm -f $CONTAINER_NAME || true"
                sh "docker run -d -p ${PORT}:80 --name $CONTAINER_NAME $IMAGE_NAME"
            }
        }

        stage('Check Running Container') {
            steps {
                echo "✅ 현재 실행 중인 컨테이너 목록:"
                sh "docker ps"
            }
        }
    }

    post {
        success {
            echo "✅ 배포 성공!"
        }
        failure {
            echo "❌ 배포 실패!"
        }
    }
}
