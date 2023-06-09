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
                sh './mvnw clean package -DskipTests=true'
            }
        }
        stage('Results') {
            steps {
//                 junit '**/target/surefire-reports/TEST-*.xml'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
        stage('Build image') {
            steps {
                sh './mvnw spring-boot:build-image -Dspring-boot.build-image.imageName=349495027745.dkr.ecr.ap-northeast-2.amazonaws.com/cicd-app:latest'
            }
        }
        stage('Push image') {
            steps {
                script {
                    sh 'rm  ~/.dockercfg || true'
                    sh 'rm ~/.docker/config.json || true'

                    docker.withRegistry('https://349495027745.dkr.ecr.ap-northeast-2.amazonaws.com/cicd-app', 'ecr:ap-northeast-2:ecr-user') {
                        sh 'docker push 349495027745.dkr.ecr.ap-northeast-2.amazonaws.com/cicd-app:latest'
                    }
                }
            }
        }
        stage('Kubernetes deploy') {
            steps {
                withKubeConfig([credentialsId: 'kubectl-credentials', serverUrl: 'https://14CF4CE3744C1CBC7228DB21707FF4A3.gr7.ap-southeast-2.eks.amazonaws.com', clusterName: 'dev-cluster']) {
                    sh 'kubectl apply -f k8s.yaml'
                }
            }
        }
    }
}