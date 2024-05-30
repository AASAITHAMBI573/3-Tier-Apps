pipeline {
    agent any

    tools{
        nodejs 'node21'
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
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

        stage('SonarQube'){
            steps{
                withSonarQubeEnv('sonar') {
                sh "$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectKey=Campground -Dsonar.projectName=Campground"
                }
            }
        }

        stage('Docker Build & Tag'){
            steps{
                script {
                withDockerRegistry(credentialsId: 'docker-pass', toolName: 'docker') {
                    sh "docker push aasaithambi5/3-tier-app:latest"
                    }
                }
            }
        }

        stage('Trivy Image Scan'){
            steps{
                sh "trivy image --format table -o fs-report.txt aasaithambi5/3-tier-app:latest"
            }
        }

        stage('Docker Push Image'){
            steps{
                script {
                    withDockerRegistry(credentialsId: 'docker-pass', toolName: 'docker') {
                        sh "docker push aasaithambi5/3-tier-app:latest"
                    }
                }
            }
        }

        stage('Docker Deploy To Local') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-pass', toolName: 'docker') {
                        sh "docker run -d -p 3000:3000 aasaithambi5/3-tier-app:latest"
                    }
                }
            }
        }
    }
}
