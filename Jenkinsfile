pipeline {
    agent any
    environment {
        DOCKER_HUB_LOGIN = credentials('docker-hub')
        REGISTRY = 'jdinucci'
    }
    stages {
        stage('Constuir imagen') {
            steps {
                sh 'docker build -t hola-mundo:v1 .'
            }
        }
        stage('Subir o pushear a docker hub') {
            steps {
                sh '''
                docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW
                docker tag hola-mundo:v1 $REGISTRY/hola-mundo:v1
                docker push $REGISTRY/hola-mundo:v1
                '''
            }
        }
    }
}