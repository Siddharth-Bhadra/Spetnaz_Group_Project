pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'echo "Compiling application..."'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo "Executing tests..."'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the application...'
                sh 'echo "Deploying to environment..."'
            }
        }
    }
}
