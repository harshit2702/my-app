pipeline {
    agent any
    environment {
        dockerimagename = "panduharsh/my-app"
        registryCredential = 'dockerhub-credentials'
        kubeconfigId = 'kubeconfig'
    }
    stages {
        stage('Checkout Source') {
            steps {
                git 'https://github.com/harshit2702/my-app.git'
            }
        }
        stage('Build Image') {
            steps {
                script {
                    dockerImage = docker.build dockerimagename
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                kubernetesDeploy(
                    configs: "kubernetes/deployment.yaml, kubernetes/service.yaml",
                    kubeconfigId: kubeconfigId
                )
            }
        }
    }
}
