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
        stage('Deploy - Staging') {
            steps {
                timeout(time: 3, unit: 'MINUTES') {
                    retry(5) {
                        sh 'npm run start'
                        echo 'deploy staging'
                    }
                }
            }
        }
        stage('e2e Test') {
            steps {
                echo 'Run e2e test on stg.'
                sh 'npm run ci'
            }
        }
        stage('Deploy - Production') {
            steps {
                echo 'deploy production'
            }
        }
    }
    post {
        always {
            echo 'This will always run'
            archiveArtifacts artifacts: 'cypress/videos/**/*.mp4', fingerprint: true
            echo 'saved artifact'
            deleteDir() /* clean up our workspace */
        }
        success {
            echo 'This will run only if successful'
            mail to: 'yong@asial.co.jp',
                 subject: "Success: ${currentBuild.fullDisplayName}",
                 body: "Url ${env.BUILD_URL}"
            slackSend channel: '#m-bot-jenkins',
                  color: 'good',
                  message: "The pipeline ${currentBuild.fullDisplayName} completed successfully."
        }
        failure {
            echo 'This will run only if failed'
            mail to: 'yong@asial.co.jp',
                 subject: "Failed: ${currentBuild.fullDisplayName}",
                 body: "Url ${env.BUILD_URL}"
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