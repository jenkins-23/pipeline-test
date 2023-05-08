pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
    post {
        always {
            mail bcc: '', body: 'The Pipeline failed :(', cc: '', from: '', replyTo: '', subject: 'Pipeline Status', to: 'gunu.shrestha@ibm.com'
        }
    }
}
