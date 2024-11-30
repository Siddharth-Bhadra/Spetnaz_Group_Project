pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'echo "Compiling application..."'
            }
        }

        stage('SAST - SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=spetnaz'
                }
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
