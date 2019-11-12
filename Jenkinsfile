pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Hello World"'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
                sh 'npm ci'
                sh 'npm run build'
            }
        }
        stage('Deploy') {
            sh 'npm run start'
        }
        stage('Test') {
            sh 'npm run ci'
        }
    }
}