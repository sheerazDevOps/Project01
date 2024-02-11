pipeline {
    agent any

    environment {
        DOCKER_USERNAME = credentials('DOCKER_USERNAME')
        DOCKER_PASSWORD = credentials('DOCKER_PASSWORD')
        DOCKER_REGISTRY_URL = 'your-docker-registry-url'
        KUBE_CONFIG_DATA = credentials('KUBE_CONFIG_DATA')
    }

    stages {
        stage('Build and Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'DOCKER_LOGIN', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin $DOCKER_REGISTRY_URL"
                    }
                    sh 'docker build -t your-docker-image:$BUILD_NUMBER .'
                    sh 'docker push your-docker-image:$BUILD_NUMBER'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'KUBE_CONFIG_DATA', variable: 'KUBE_CONFIG_DATA')]) {
                        writeFile file: 'kubeconfig', text: "${env.KUBE_CONFIG_DATA}"
                    }
                    sh 'kubectl apply -f path/to/deployment.yaml'
                }
            }
        }
    }
}
