pipeline {
    agent any

    environment {
        IMAGE_NAME = 'harbor.aliteprojects.com/k8s-demo/flask-k8s-app:latest'
        REGISTRY = 'harbor.aliteprojects.com'
        DOCKER_IMAGE = "${IMAGE_NAME}"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Ketan34790/k8s-jenkins-poc.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                 withCredentials([usernamePassword(credentialsId: 'HARBOR_LOGIN', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh """
                            echo "Logging into Harbor securely..."
                            echo "$DOCKER_PASS" | docker login ${env.REGISTRY} -u "$DOCKER_USER" --password-stdin
                        """
                    }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                sh 'kubectl apply -f flask-deployment.yaml'
            }
        }
    }
}
