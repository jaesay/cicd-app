pipeline {
    agent any
    stages {
        stage('Git clone') {
            steps {
                git branch: 'main', url: 'https://github.com/jaesay/cicd-app'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Results') {
            steps {
                junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}