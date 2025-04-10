pipeline {
    agent any
    // agent {
    //     label 'Agent-npm'
    // }
    options {
        disableConcurrentBuilds()
    }
    environment {
        appVersion = ''
    }
    stages {
        stage('Read Version') {
            steps {
                script {
                    def packageJson = readJSON file: 'code/package.json'
                    appVersion = packageJson.version
                    echo "App Version: ${appVersion}"
                }
            }
        }
        stage('Docker Build') {
            steps {
                sh """
                docker build -t mahiketh/backend:${appVersion} .
                docker images
                """
            }
        }
    }
    post {
        always {
            deleteDir()
        }
    }
}
