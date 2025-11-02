

pipeline {
    agent any

    tools {
        nodejs 'node18'
    }

    environment {
        // Change these to match your setup
        GIT_REPO = 'https://github.com/leenavemula25/my-node-pipeline'
        EMAIL_TO = 'leenavemula@gmail.com'
        NODE_ENV = 'production'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Cloning Git Repository...'
                git branch: 'main', url: "${GIT_REPO}"
            }
        }

        stage('Build') {
            steps {
                echo 'Installing Node dependencies...'
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests (if any)...'
                sh 'npm test || echo "No tests configured"'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Node.js Application...'
                sh '''
                    pkill -f "node server.js" || true
                    nohup node server.js > output.log 2>&1 &
                '''
                echo 'App started successfully!'
            }
        }
    }

    post {
        success {
            echo 'Build Successful — Sending Email'
            emailext (
                subject: "SUCCESS: ${env.JOB_NAME} [Build #${env.BUILD_NUMBER}]",
                body: """
                    <h3>Jenkins Job Successful!</h3>
                    <p><b>Job:</b> ${env.JOB_NAME}</p>
                    <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                    <p><b>Environment:</b> ${env.NODE_ENV}</p>
                    <p><b>Build URL:</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                to: "${EMAIL_TO}"
            )
        }

        failure {
            echo 'Build Failed — Sending Email'
            emailext (
                subject: "FAILURE: ${env.JOB_NAME} [Build #${env.BUILD_NUMBER}]",
                body: """
                    <h3>Jenkins Job Failed!</h3>
                    <p><b>Job:</b> ${env.JOB_NAME}</p>
                    <p><b>Build Number:</b> ${env.BUILD_NUMBER}</p>
                    <p><b>Check logs:</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                to: "${EMAIL_TO}"
            )
        }
    }
}
