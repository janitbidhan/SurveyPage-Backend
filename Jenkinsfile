pipeline {
  agent {
    node {
      label 'Swe645-jenkins'
    }
  }
  environment {
    DOCKER_REGISTRY = 'docker.io'
    DOCKER_CREDENTIALS = credentials("docker-credentials")
    KUBERNETES_NAMESPACE = 'swe-a2'
    KUBERNETES_DEPLOYMENT_NAME = 'deploy-a2'
    KUBERNETES_CONTAINER_NAME = 'container-0'
    KUBERNETES_CONTAINER_PORT = 8080
  }
  stages {
    stage('Prerequisites') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          echo "Username: ${USERNAME}"
          echo "Password: ${PASSWORD}"
        }
      }
    }
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh 'rm -rf *.war'
        sh 'jar -cvf ROOT.war -C src/main/webapp .'
      }
      post {
        success {
          archiveArtifacts artifacts: '*.war', fingerprint: true
        }
      }
    }
    stage('Docker Build and Push') {
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            sh 'echo ${PASSWORD} | sudo docker login -u ${USERNAME} --password-stdin'
            sh 'sudo docker build -t "bidhanjanit/swe-assignment2" .'
            sh 'sudo docker push bidhanjanit/swe-assignment2'
          }
        }
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        script {
             sh "kubectl rollout restart deploy ${KUBERNETES_DEPLOYMENT_NAME} -n ${KUBERNETES_NAMESPACE} --v=9"
        }
      }
    }
  }
}
