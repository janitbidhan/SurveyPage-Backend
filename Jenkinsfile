// //dckr_pat__snbNu8AUrQA_pX-9FWkz7UYTCw
// pipeline {
//     agent any
//     environment {
//         DOCKER_CREDENTIAL = credentials('DOCKER_CREDENTIAL')
//     }
//     stages {
//         stage('Stage 1: Building the WAR file and docker image') {
//             steps {
//                 script {
//                     checkout scm
//                     sh 'rm -rf *.war'
//                     sh 'jar -cvf ROOT.war -C src/main/webapp . '
//                     sh 'echo ${BUILD_TIMESTAMP}'
//                     sh 'docker login -u bidhanjanit -p ${DOCKER_CREDENTIAL}'
//                     def customImage = docker.build("bidhanjanit/swe-assignment2:${BUILD_TIMESTAMP}")
//                     sh "docker push bidhanjanit/swe-assignment2:${BUILD_TIMESTAMP}"
//                 }
//             }
//         }
//     }
// }

pipeline {
  agent any
  environment {
    DOCKER_REGISTRY = 'docker.io'
    DOCKER_CREDENTIALS = credentials("docker-credentials")
    KUBERNETES_NAMESPACE = 'default'
    KUBERNETES_DEPLOYMENT_NAME = 'myapp'
    KUBERNETES_CONTAINER_NAME = 'myapp'
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
            sh 'echo ${PASSWORD} | docker login -u ${USERNAME} --password-stdin'
            def timestamp = new Date().format('yyyyMMddHHmmss')
            def image = docker.build("bidhanjanit/swe-assignment2:${timestamp}", '.')
            image.push()
          }
        }
      }
    }
   stage('Deploy to Kubernetes') {
      steps {
        script {
          withKubeConfig([credentialsId: 'kubeconfig']) {
            sh 'kubectl set image deployment/${KUBERNETES_DEPLOYMENT_NAME} ${KUBERNETES_CONTAINER_NAME}=${DOCKER_REGISTRY}/myapp:${timestamp} -n ${KUBERNETES_NAMESPACE}'
          }
        }
      }
    }
  }
}









