pipeline {
  agent {
    kubernetes {
      label 'docker'
      defaultContainer 'docker'
    }
  }
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
        sh 'docker build -t panduharsh/my-app .'
      }
    }
    stage('Push Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh '''
            echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
            docker push panduharsh/my-app
          '''
        }
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG_FILE')]) {
          sh '''
            export KUBECONFIG=$KUBECONFIG_FILE
            kubectl apply -f kubernetes/deployment.yaml
            kubectl apply -f kubernetes/service.yaml
          '''
        }
      }
    }
  }
}
