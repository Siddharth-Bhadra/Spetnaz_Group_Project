pipeline {
    agent any
    tools {
        maven 'Maven' // Use the name you configured in Jenkins
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh 'echo "Compiling application..."'
            }
        }
        stage('Provision Infrastructure') {
            steps {
                echo 'Provisioning infrastructure with Vagrant...'
                sh 'vagrant up'
            }
        }
        stage('SAST - SonarQube Analysis') {
            steps {
                withSonarQubeEnv('spetnaz') {
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
