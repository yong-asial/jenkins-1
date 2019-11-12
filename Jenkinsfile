pipeline {
    agent {
        label '!Windows'
    }
    environment {
        ENV1 = 'true'
        ENV2 = 'data'
    }
    stages {
        stage('Use Env') {
            steps {
                echo "ENV1 is ${ENV1}"
                echo "ENV2 is ${ENV2}"
                sh 'printenv'
            }
        }
        stage('System Info') {
            steps {
                sh 'pwd'
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
                sh 'npm run ci'
            }
        }
    }
    post {
        always {
            echo 'This will always run'
            archiveArtifacts artifacts: 'cypress/videos/**/*.mp4', fingerprint: true
            echo 'saved artifact'
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