pipeline {
    agent any
    tools {nodejs "NodeJS"}
    environment{
        JOB_NAME = "${env.JOB_NAME}"
    }
    stages{
        stage("Git cloning"){
            steps {
                git 'https://github.com/Marsh-sudo/gallery-1.git/'
            }
        }
        stage('install dependancies'){
            steps{
                sh "echo 'Job Name: $JOB_NAME'"
                sh 'npm install'
            }
        }
        stage('building'){
            steps{
                sh 'echo "Building $JOB_NAME"'
                sh 'npm run'
            }
        }
        stage('testing'){
            steps{
                sh 'echo "Running tests for: $JOB_NAME"'
                sh 'npm test'
            }
        }
        stage('deploying'){
            steps {
                withCredentials([usernameColonPassword(credentialsId: 'heroku', variable: 'HEROKU_CREDENTIALS' )]){
                    sh 'git push https://${HEROKU_CREDENTIALS}@git.heroku.com/'
                }
            }
        }
    }
    post{
        success{
            slackSend channel: 'munyaokelvin_ip1', color: 'good', iconEmoji: '👍🏾', message: 'Status of ${env.JOB_NAME} has ran successfully. ${env.BUILD_NUMBER} ${env.BUILD_URL}'
        }
        failure{
            slackSend channel: 'munyaokelvin_ip1', color: 'danger', iconEmoji: '😖', message: 'Status of ${env.JOB_NAME} has failed. ${env.BUILD_NUMBER} ${env.BUILD_URL}'
            emailext body: 'Status of ${env.JOB_NAME} has failed to build. ${env.BUILD_NUMBER} ${env.BUILD_URL}', subject: 'Failure of Pipeline', to: 'samuel.kadima@moringaschool.com, marshdeveloper99@gmail.com'
        }
    }
}