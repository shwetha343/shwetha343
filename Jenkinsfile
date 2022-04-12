pipeline {
    agent any 
    environment {
    DOCKERHUB_CREDENTIALS = credentials('jenkins_docker_id')
    }
    stages { 
        stage('SCM Checkout') {
            steps{
            git credentialsId: 'GitHubId', url: 'https://github.com/shwetha343/shwetha343.git'
            }
        }

        stage('Build docker image') {
            steps {  
                sh 'docker build -t shwetha/nodeapp:$BUILD_NUMBER .'
            }
        }
        stage('login to dockerhub') {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh 'docker push shwetha/nodeapp:$BUILD_NUMBER'
            }
        }
}
post {
        always {
            sh 'docker logout'
        }
    }
}
