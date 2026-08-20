pipeline {

    agent {
        label 'node2'
    }

    environment {
        // Replace with your actual Docker Hub username
        DOCKER_IMAGE = 'shubhamnakade/my-jenkins-app'
        CREDENTIALS_ID = 'Docker'
    }

    stages {

        stage('Checkout') {
            steps {
                echo '📦 Jenkins is checking out source code from GitHub...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                    echo "========================================="
                    echo "🐳 Building Docker Image"
                    echo "========================================="
                    docker build -t ${DOCKER_IMAGE}:latest .
                '''
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${CREDENTIALS_ID}", 
                                                  usernameVariable: 'DOCKER_USER', 
                                                  passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "========================================="
                        echo "🚀 Pushing to Docker Hub"
                        echo "========================================="
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${DOCKER_IMAGE}:latest
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    echo "========================================="
                    echo "🚀 Deploying Application"
                    echo "========================================="
                    docker pull ${DOCKER_IMAGE}:latest
                    docker stop project5-app || true
                    docker rm project5-app || true
                    docker run -d -p 8081:80 --name project5-app ${DOCKER_IMAGE}:latest
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
                    curl -f http://localhost:8081 || curl -f http://43.204.235.143:8081
                '''
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline completed successfully!'
        }
        failure {
            echo '❌ Pipeline failed.'
        }
    }
}
