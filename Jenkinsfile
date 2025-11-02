pipeline {
    agent any

    tools {
        nodejs 'node18'   // Must match the name in Manage Jenkins → Tools
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code...'
            }
        }

        stage('Build App') {
            steps {
                echo "Installing dependencies..."
                sh 'npm install'
            }
        }

        stage('Deploy Locally') {
            steps {
                echo 'Starting the app...'
                sh 'bash restart.sh'
            }
        }
    }

    ppipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                sh 'npm install'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }

    post {
        success {
            emailext (
                subject: "Jenkins Job '${env.JOB_NAME}' SUCCESS [Build #${env.BUILD_NUMBER}]",
                body: """
                <p>Good news!</p>
                <p>Your Jenkins job <b>${env.JOB_NAME}</b> completed successfully </p>
                <p><b>Build URL:</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                to: 'leenavemula@gmail.com'
            )
        }

        failure {
            emailext (
                subject: "Jenkins Job '${env.JOB_NAME}' FAILED [Build #${env.BUILD_NUMBER}]",
                body: """
                <p>Uh oh!</p>
                <p>Your Jenkins job <b>${env.JOB_NAME}</b> failed </p>
                <p><b>Check logs here:</b> <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """,
                to: 'leenavemula@gmail.com'
            )
        }
    }
}

}
