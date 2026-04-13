pipeline {
    agent any

    environment {
        EMAIL_TO = "abhyudayantaal@gmail.com"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/AbhyudayAntaal/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
            post {
                always {
                    emailext(
                        to: "${EMAIL_TO}",
                        subject: "Test Stage Result: ${currentBuild.currentResult}",
                        body: "The test stage has completed.",
                        attachLog: true
                    )
                }
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
            post {
                always {
                    emailext(
                        to: "${EMAIL_TO}",
                        subject: "Security Scan Result: ${currentBuild.currentResult}",
                        body: "Security scan has completed.",
                        attachLog: true
                    )
                }
            }
        }
    }
}