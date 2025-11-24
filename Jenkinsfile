pipeline {
    agent any

    tools {
        nodejs "node-lts"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm ci || npm install'
            }
        }

        stage('Run tests') {
            steps {
                sh 'npm test'
            }
        }
    }
}
