pipeline {
    agent any

    tools{
        nodejs 'node21'
    }

    stages{
        stage('Git Checkout'){
            steps{
                git branch: 'main', credentialsId: 'git-token', url: 'https://github.com/AASAITHAMBI573/3-Tier-Apps.git'
            }
        }

        stage('Install Package Dependencies'){
            steps{
                sh "npm install"
            }
        }

        stage('Unit Tests'){
            steps{
                sh "npm test"
            }
        }

        stage('Trivy FS Scan'){
            steps{
                sh "trivy fs --format table -o fs-report.html ."
            }
        }
    }
}
