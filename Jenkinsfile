pipeline {
    agent any

    environment {
        NODE_HOME = "C:\\Program Files\\nodejs"  
        PATH = "${env.NODE_HOME};${env.PATH}"
        IMAGE_NAME = "reactinho"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/youssef-salih/devopsFront.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '''
                docker build -t %IMAGE_NAME% .
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                bat '''
                docker stop %IMAGE_NAME% || echo "No running container"
                docker rm %IMAGE_NAME% || echo "No container to remove"
                docker run -d -p 3000:80 --name %IMAGE_NAME% %IMAGE_NAME%
                '''
            }
        }
    }

    post {
        success {
            echo '✅ React app built and running in Docker!'
        }
        failure {
            echo '❌ Build or Docker failed!'
        }
    }
}
