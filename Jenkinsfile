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
                    docker images my-jenkins-app
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    echo "========================================="
                    echo "🚀 Deploying Application"
                    echo "========================================="

                    docker stop project3-app || true
                    docker rm project3-app || true

                    docker run -d \
                        -p 8080:80 \
                        --name project3-app \
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

                    curl http://localhost:8080

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
            echo '🎉 Project 3 CI/CD Pipeline completed successfully!'
        }

        failure {
            echo '❌ Project 3 Pipeline failed. Check the console output.'
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
