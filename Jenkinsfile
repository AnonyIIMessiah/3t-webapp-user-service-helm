pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }
    environment {
        DOCKER_PASS = credentials('DOCKER_PASS')
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    stages {
        stage('Preparation') {
            steps {
                script {
                    // Clean workspace before starting
                    sh 'apt-get update && apt-get install -y docker.io'
                }
            }
        }

        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }
        stage('Build') {
            steps {
                dir('user-service') {
                    script {
                        sh 'docker build -t demoniiexe/microservice-user:"${IMAGE_TAG}" .'
                    }
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    
                        // Login to Docker Hub
                        sh 'docker login -u demoniiexe -p $DOCKER_PASS'
                        sh 'docker push demoniiexe/microservice-user:"${IMAGE_TAG}"'
                    
                }
            }
        }
        stage('EKS-setup') {
            steps {
                script {
                    sh 'aws eks update-kubeconfig --name task-eks --region ap-south-1'
                }
            }
        }
        stage('Deploy') {
            
            steps {
                    script {
                        def releaseName = "user-service"
                        def namespace = "default"

                        def isInstalled = sh(
                            script: "helm list -n ${namespace} -q | grep -w ${releaseName} || true",
                            returnStdout: true
                        ).trim()

                        if (isInstalled) {
                            echo "Helm release '${releaseName}' is already installed."
                            sh 'helm upgrade user-service user-service-helm --set deployment.image.tag="${IMAGE_TAG}"'
                        } else {
                            echo "Helm release '${releaseName}' is not installed. Installing now..."
                            sh 'helm install user-service user-service-helm --set deployment.image.tag="${IMAGE_TAG}"'
                        }
                    }
            }   
        }
    }
}
