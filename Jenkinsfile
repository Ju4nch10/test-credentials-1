pipeline {
    agent any
    environment {
        DOCKER_HUB_LOGIN = credentials('docker-hub')
        REGISTRY = 'jdinucci'
        IMAGE = 'hello-world'
    }
    stages {
        stage('Constuir imagen') {
            steps {
                sh '''
                VERSION=$(jq --raw-output .version package.json)
                echo $VERSION >version.txt
                docker build -t $IMAGE:$(cat version.txt) .
                '''
            }
        }
        stage('Subir o pushear a docker hub') {
            steps {
                sh '''
                docker login --username=$DOCKER_HUB_LOGIN_USR --password=$DOCKER_HUB_LOGIN_PSW
                docker tag $IMAGE:$(cat version.txt) $REGISTRY/$IMAGE:$(cat version.txt)
                docker push $REGISTRY/$IMAGE:$(cat version.txt)
                '''
            }
        }
    }
}