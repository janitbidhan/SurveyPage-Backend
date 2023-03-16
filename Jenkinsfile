//dckr_pat_p-FpvhOE2NcXQqZlw4lnQKKnxgc
pipeline {
    agent any
    environment {
        DOCKER_CREDENTIAL = credentials('DOCKER_CREDENTIAL')
    }
    stages {
        stage('Stage 1: Building the WAR file and docker image') {
            steps {
                script {
                    checkout scm
                    sh 'rm -rf *.war'
                    sh 'jar -cvf ROOT.war -C src/main/webapp . '
                    sh 'echo ${BUILD_TIMESTAMP}'
                    sh 'docker login -u bidhanjanit -p ${DOCKER_CREDENTIAL}'
                    def customImage = docker.build("bidhanjanit/swe-assignment2:${BUILD_TIMESTAMP}")
                    sh "docker push bidhanjanit/swe-assignment2:${BUILD_TIMESTAMP}"
                }
            }
        }
        stage('Stage 2: Pushing the image to DockerHub') {
            steps {
                script {
                    sh "docker push bidhanjanit/swe-assignment2:${BUILD_TIMESTAMP}"
                }
            }
        }
//         stage('Stage 3: Restarting the deployment to pull the latest image') {
//             steps {
//                 sh 'kubectl rollout restart deploy cluster-a2 -n assn2'
//             }
//         }
    }
}
