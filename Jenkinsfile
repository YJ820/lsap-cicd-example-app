pipeline {
    agent any

    tools {
        nodejs 'node-lts'
    }

    environment {
        GIT_URL = 'https://github.com/YJ820/lsap-cicd-example-app.git'
        DISCORD_WEBHOOK = credentials('chatops-webhook')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo "Branch: ${env.BRANCH_NAME}"
            }
        }

        stage('Static Analysis') {
            steps {
                sh 'npm ci || npm install'
                sh 'npm run lint'
            }
        }
    }

    post {
        failure {
            script {
                def msg = """IM3014 CI/CD Build Failed
Name: 陳予婕
Student ID: B12705038
Job: ${env.JOB_NAME}
Build: ${env.BUILD_NUMBER}
Repo: ${GIT_URL}
Branch: ${env.BRANCH_NAME}
Status: ${currentBuild.currentResult}
"""

                // 把字串變成安全的 JSON
                def escaped = msg.replace("\\", "\\\\")
                                 .replace('"', '\\"')
                                 .replace("\n", "\\n")
                def payload = '{"content": "' + escaped + '"}'

                sh """
                  curl -X POST \
                    -H 'Content-Type: application/json' \
                    -d '${payload}' \
                    ${DISCORD_WEBHOOK}
                """
            }
        }
    }
}
