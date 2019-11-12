pipeline {
    agent any
    stages {
        stage('System Info') {
            steps {
                sh 'node -v'
                sh 'npm -v'
            }
        }
        stage('Build') {
            steps {
                sh 'echo "Hello World"'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
                sh 'npm ci'
                sh 'npm run build'
                retry(3) {
                    sh 'echo "retry 3 times"'
                }
                timeout(time: 3, unit: 'MINUTES') {
                    sh 'echo "timeout for 3mn"'
                }
            }
        }
        stage('Deploy') {
            steps {
                timeout(time: 3, unit: 'MINUTES') {
                    retry(5) {
                        sh 'npm run start'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                sh 'npm run ci; exit 1'
            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}