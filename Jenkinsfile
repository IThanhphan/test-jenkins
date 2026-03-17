pipeline {
    agent any

    stages {

        stage('Install') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker') {
            steps {
                sh 'docker build -t jenkins-demo .'
            }
        }

        stage('Deploy Staging') {
            steps {
                sh '''
                docker stop staging-app || true
                docker rm staging-app || true
                docker run -d -p 4000:3000 --name staging-app jenkins-demo
                '''
            }
        }

        stage('Approve Production') {
            steps {
                input "Deploy to production?"
            }
        }

        stage('Deploy Production') {
            steps {
                sh '''
                docker stop prod-app || true
                docker rm prod-app || true
                docker run -d -p 5000:3000 --name prod-app jenkins-demo
                '''
            }
        }
    }
}