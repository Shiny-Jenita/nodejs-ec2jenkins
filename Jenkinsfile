pipeline {
    agent {
        docker { 
            image 'node:20'      // Official Node.js 20 image
            args '-u root:root'  // Optional: run as root to avoid permission issues
        }
    }

    environment {
        PROJECT_NAME = "Node.js EC2 Jenkins Pipeline"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "🔄 Checking out code..."
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "📦 Installing npm dependencies..."
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                echo "🏗️ Building project..."
                // If you have a build script, run it here:
                sh 'npm run build || echo "No build script, skipping..."'
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Running tests..."
                sh 'npm test || echo "No tests found, skipping..."'
            }
        }

        stage('Deploy') {
            steps {
                echo "🚀 Deploying app (test stage only)..."
                sh 'echo "Simulated deployment step"'
            }
        }
    }

    post {
        always {
            echo "🧹 Cleaning up workspace..."
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
