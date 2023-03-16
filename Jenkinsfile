pipeline {
    agent any
    environment {
        DOCKER_CREDENTIAL = credentials('dckr_pat_p-FpvhOE2NcXQqZlw4lnQKKnxgc')
    }
    stages {
        stage('Building the WAR file and docker image') {
            steps {
                script {
                    checkout scm
                    sh 'rm -rf *.war'
                    sh 'jar -cvf ROOT.war -C src/main/webapp . '
                    sh 'echo ${BUILD_TIMESTAMP}'
                    sh 'docker login -u bidhanjanit -p ${DOCKERHUB_PASS}'
                    def customImage = docker.build("bidhanjanit/swe-assignment2:${BUILD_TIMESTAMP}")
                }
            }
        }
        stage('Pushing the image to DockerHub') {
            steps {
                script {
                    sh 'docker push bidhanjanit/swe-assignment2:${BUILD_TIMESTAMP}'
                }
            }
        }
        stage('Restarting the deployment to pull the latest image') {
            steps {
                sh 'kubectl rollout restart deploy cluster-a2 -n assn2'
            }
        }
    }
}
