pipeline {
    agent any

    stages{
        stage("git SCM Checkout"){
            steps{
                git branch: 'main', credentialsId: 'git-token', url: 'https://github.com/AASAITHAMBI573/3-Tier-Apps.git'
            }
        }
    }
}
