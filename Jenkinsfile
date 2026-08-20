pipeline {

    agent {
        label 'node2'
    }

    stages {

        stage('Checkout') {
            steps {
                echo '📦 Jenkins is checking out source code from GitHub...'
                checkout scm
            }
        }

        stage('Verify Source Code') {
            steps {
                sh '''
                    echo "========================================="
                    echo "📂 Source Code"
                    echo "========================================="

                    echo "Current directory:"
                    pwd

                    echo ""
                    echo "Files in workspace:"
                    ls -la

                    echo ""
                    echo "Git commit:"
                    git log -1 --oneline
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    echo "========================================="
                    echo "🐳 Building Docker Image"
                    echo "========================================="

                    docker build \
                        -t my-jenkins-app:latest \
                        .

                    echo ""
                    echo "Docker image created:"
                    docker images my-jenkins-app:latest
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    echo "========================================="
                    echo "🚀 Deploying Application"
                    echo "========================================="

                    docker stop project5-app || true
                    docker rm project5-app || true

                    docker run -d \
                        -p 8081:80 \
                        --name project5-app \
                        my-jenkins-app:latest

                    echo ""
                    echo "Running container:"
                    docker ps
                '''
            }
        }

        stage('Test Application') {
            steps {
                sh '''
                    echo "========================================="
                    echo "🌐 Testing Application"
                    echo "========================================="

                    sleep 3

                    curl http://localhost:8081

                    echo ""
                    echo "========================================="
                    echo "✅ Application is working!"
                    echo "========================================="
                '''
            }
        }
    }

    post {

        success {
            echo '🎉 Project 5 CI/CD Pipeline completed successfully!'
        }

        failure {
            echo '❌ Project 5 Pipeline failed. Check the console output.'
        }

        always {
            sh '''
                echo "========================================="
                echo "🐳 Running Containers"
                echo "========================================="

                docker ps || true
            '''
        }
    }
}
