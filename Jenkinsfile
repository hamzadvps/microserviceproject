pipeline {

    agent { label 'micro3' }

    environment {
        DOCKERHUB_CREDENTIALS = 'mydockerhub_cred'
        DOCKERHUB_USERNAME = 'hamzadvps'
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/hamzadvps/microserviceproject.git'
            }
        }

        stage('Build Auth Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_USERNAME/authservice:latest ./authservice'
            }
        }

        stage('Build Product Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_USERNAME/productservice:latest ./productservice'
            }
        }

        stage('Build Order Image') {
            steps {
                sh 'docker build -t $DOCKERHUB_USERNAME/orderservice:latest ./orderservice'
            }
        }

        stage('Docker Login') {
            steps {

                withCredentials([usernamePassword(
                    credentialsId: 'mydockerhub_cred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'

                }
            }
        }

        stage('Push Images') {
            steps {

                sh 'docker push $DOCKERHUB_USERNAME/authservice:latest'
                sh 'docker push $DOCKERHUB_USERNAME/productservice:latest'
                sh 'docker push $DOCKERHUB_USERNAME/orderservice:latest'

            }
        }
    }
}