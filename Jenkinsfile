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
            steps {
                sh 'npm run start'                
            }
        }
        stage('Test') {
            steps {
                sh 'npm run ci'
            }
        }
    }
}