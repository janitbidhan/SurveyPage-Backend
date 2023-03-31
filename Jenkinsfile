pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_CREDENTIALS = credentials("docker-credentials")
        KUBERNETES_NAMESPACE = 'namespace-survey-a2'
        KUBERNETES_DEPLOYMENT_NAME = 'deployment-survey'
        KUBERNETES_CONTAINER_NAME = 'container-survey'
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
                        def timestamp = new Date().format('yyyyMM')
                        def image = docker.build("bidhanjanit/swe-assignment2:${timestamp}", '.')
                        image.push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withKubeConfig(
                            credentialsId: 'gke-creds',
                            clusterName: 'surveycluster',
                            zone: 'us-central1-c',
                            project: 'swe-645-assignment2'
                    ) {
                        def timestamp = new Date().format('yyyyMM')
                        //sh "kubectl set image deployment/${KUBERNETES_DEPLOYMENT_NAME} ${KUBERNETES_CONTAINER_NAME}=${DOCKER_REGISTRY}/bidhanjanit/swe-assignment2:${timestamp} -n ${KUBERNETES_NAMESPACE}"
                        sh "kubectl rollout restart deploy ${KUBERNETES_DEPLOYMENT_NAME} -n ${KUBERNETES_NAMESPACE} --v=9"
                    }
                }
            }
        }
    }
}
