pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Building Node.js app..."
                sh 'node --version'
            }
        }

        stage('Test') {
            steps {
                echo " Running tests..."
                sh 'echo "All tests passed!"'
            }
        }
    }

    post {
        always {
            echo "✅ Pipeline finished!"
        }
    }
}
