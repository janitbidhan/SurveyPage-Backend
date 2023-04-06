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
            def timestamp = new Date().format('yyyyMM')
            sh 'sudo docker build -t "bidhanjanit/swe-assignment2" .'
            //                         def image = docker.build("bidhanjanit/swe-assignment2:${timestamp}", '.')
            sh 'sudo docker push bidhanjanit/swe-assignment2'
            //                         image.push()
          }
        }
      }
    }
    stage('Deploy to Rancher') {
      steps {
        script {
            sh 'curl -sfL https://get.rancher.io | sudo sh -'
            sh 'sudo rancher login https://18.209.26.76/ -t Rancher@12345'
            sh 'sudo rancher kubectl apply -f k8s-deployment.yaml --namespace=${KUBERNETES_NAMESPACE}'
            sh 'sudo rancher kubectl rollout restart deployment/${KUBERNETES_DEPLOYMENT_NAME} --namespace=${KUBERNETES_NAMESPACE}'
        }
      }
    }
    // stage('Deploy to Kubernetes') {
    //   steps {
    //     script {
    //       withKubeConfig(
    //         credentialsId: 'gke-creds',
    //         clusterName: 'cluster-swe',
    //         zone: 'us-central1-c',
    //         project: 'swe-645-assignment2'
    //       ) {
    //         //                         def timestamp = new Date().format('yyyyMM')
    //         //                         sh "kubectl set image deployment/${KUBERNETES_DEPLOYMENT_NAME} ${KUBERNETES_CONTAINER_NAME}=${DOCKER_REGISTRY}/bidhanjanit/swe-assignment2:${timestamp} -n ${KUBERNETES_NAMESPACE} --v=9"
    //         //                         sh "kubectl rollout restart deploy ${KUBERNETES_DEPLOYMENT_NAME} -n ${KUBERNETES_NAMESPACE} --v=9"
    //       }
    //     }
    //   }
    // }
  }
}
