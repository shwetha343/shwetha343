pipeline {
    agent any 
    environment {
    DOCKERHUB_CREDENTIALS = credentials('jenkins_docker_id')
    }
    stages { 
        stage('SCM Checkout') {
            steps{
            git branch: 'main', credentialsId: 'GitHubId', url: 'https://github.com/shwetha343/shwetha343.git'
            }
        }

        stage('Build docker image') {
            steps {  
                sh 'docker build -t shwe9876/nodeapp:$BUILD_NUMBER .'
            }
        }
        
        stage('SonarQube analysis') {
            def scannerHome = tool 'SonarScanner 4.0';
            withSonarQubeEnv('My SonarQube Server') { // If you have configured more than one global server connection, you can specify its name
            sh "${scannerHome}/bin/sonar-scanner"
            }
        }
        
        stage('login to dockerhub') {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh 'docker push shwe9876/nodeapp:$BUILD_NUMBER'
            }
        }
}
post {
        always {
            sh 'docker logout'
        }
    }
}
