    pipeline {
        agent any 

        environment {
            DOCKER_IMAGE = "shivaavvari/flask_demo"
            DOCKER_CREDENTIAL_ID = "1abf8fcd-c15e-4dae-a7da-056e6834ed40" // ID of your Docker Hub credentials in Jenkins
        }

        stages {
            stage('Checkout Code') {
                steps {
                    git url: 'https://github.com/shivaavvari/demo.git', branch: 'main' // Replace with your repo URL and branch
                }
            }

            stage('Build Docker Image') {
                steps {
                    script {
                        sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ."
                        sh "docker tag ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ${DOCKER_IMAGE}:latest"
                    }
                }
            }

            stage('Push to Docker Hub') {
                steps {
                    script {
                        withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIAL_ID}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                            sh' echo "$DOCKER_PASSWORD" | docker login --username "$DOCKER_USERNAME" --password-stdin '
                            sh " docker push ${DOCKER_IMAGE}:${env.BUILD_NUMBER}"
                            sh " docker push ${DOCKER_IMAGE}:latest"
                            sh " docker logout" // Optional: Logout for security
                        }
                    }
                }
            }
        }
    }
