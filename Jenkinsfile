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
                    docker build -t 339712874850.dkr.ecr.us-east-1.amazonaws.com/expense/dev/backend:${appVersion} .
                    docker images
                """
            }
        }
        stage('Docker Push') {
            steps {
                withAWS(region: "${awsRegion}", credentials: "${awsCreds}") {
                    sh """
                        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 339712874850.dkr.ecr.us-east-1.amazonaws.com
                        docker push 339712874850.dkr.ecr.us-east-1.amazonaws.com/expense/dev/backend:${appVersion}                        
                    """
                }
            }
        }
    }
    post {
        always {
            deleteDir()
        }
    }
}
