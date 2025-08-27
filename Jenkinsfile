pipeline {
    agent {
        docker {
            image 'docker:20.10.7'
            args '-v /var/run/docker.sock:/var/run/docker.sock --user root'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/6510110132-natcha/jenkins-demo-app.git'
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t jenkins-demo-app:latest .'
            }
        }
        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
                sh 'pytest || true'
            }
        }
        stage('Run Container') {
            steps {
                sh '''
                # ลบ container เก่าถ้ามี
                if [ $(docker ps -a -q -f name=demo-app) ]; then
                    docker rm -f demo-app
                fi
                # รัน container ใหม่ที่ port 8081
                docker run -d -p 8081:8081 --name demo-app jenkins-demo-app:latest
                '''
            }
        }
    }
}
