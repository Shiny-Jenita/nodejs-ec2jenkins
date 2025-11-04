pipeline {
    agent any

    // Optional: environment variables for testing
    environment {
        PROJECT_NAME = "Jenkins Test Pipeline"
    }

    // Optional: Poll the repo every 5 minutes (if no webhook)
    triggers {
        pollSCM('H/5 * * * *')
    }

    stages {

        stage('Checkout') {
            steps {
                echo "🔄 Checking out source code..."
                git branch: 'main',
                    url: 'https://github.com/Shiny-Jenita/nodejs-ec2jenkins
.git',
                    credentialsId: 'github-token'
            }
        }

        stage('Build') {
            steps {
                echo "🏗️  Building project..."
                // You can replace this with your real build command
                sh 'echo "Simulating build step..."'
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Running tests..."
                // Replace with real test commands
                sh '''
                    echo "Running unit tests..."
                    sleep 2
                    echo "All tests passed!"
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying application (test stage only)..."
                sh 'echo "Simulating deployment..."'
            }
        }
    }

    post {
        always {
            echo "🧹 Cleaning workspace..."
            cleanWs()
        }
        success {
            echo "✅ ${PROJECT_NAME} completed successfully!"
        }
        failure {
            echo "❌ ${PROJECT_NAME} failed!"
        }
    }
}
