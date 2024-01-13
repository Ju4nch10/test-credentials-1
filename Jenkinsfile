pipeline {
    agent any
    environment {
        DOCKER_HUB_LOGIN = credentials('docker-hub')
        REGISTRY = 'jdinucci'
        IMAGE = 'hello-world'
    }
    stages {
        stage('init') {
            agent {
                docker {
                    image 'node:alpine'
                    args '-u root:root'
                }
            }
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:alpine'
                    args '-u root:root'
                }
            }
            steps {
                sh 'npm run test'
            }
        }
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
        stage('Reemplazar palabras') {
            steps {
                sh '''
                sed -i -- "s/REGISTRY/$REGISTRY/g" docker-compose.yml
                sed -i -- "s/IMAGE/$IMAGE/g" docker-compose.yml
                sed -i -- "s/TAG/$(cat version.txt)/g" docker-compose.yml
                cat docker-compose.yml
                '''
            }
        }
    }
}