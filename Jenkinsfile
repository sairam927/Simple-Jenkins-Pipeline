pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-demo'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sairam927/Simple-Jenkins-Pipeline.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Test') {
            steps {
                echo "Running simple test..."
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop $IMAGE_NAME || true
                docker rm $IMAGE_NAME || true
                docker run -d --name $IMAGE_NAME -p 5000:5000 $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success { echo '✅ Deployment successful!' }
        failure { echo '❌ Deployment failed!' }
    }
}
