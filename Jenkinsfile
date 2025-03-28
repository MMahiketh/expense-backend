pipeline {
    agent {
        label 'Agent-npm'
    }
    environment {
        appVersion = ''
    }
    stages {
        stage('Read Version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "App Version: ${appVersion}"
                }
            }
        }
    }
}
