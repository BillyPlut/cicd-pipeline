pipeline {
    agent any

    environment {
        
        DOCKER_IMAGE = "plutobilly/cicd-pipeline"
    }

    stages {
        stage('Git Checkout') { // Activity 1
            steps {
                checkout scm
            }
        }

        stage('Application Build') { // Activity 2
            steps {
                sh 'chmod +x scripts/build.sh'
                sh './scripts/build.sh'
            }
        }

        stage('Tests') { // Activity 3
            steps {
                sh 'chmod +x scripts/test.sh'
                sh './scripts/test.sh'
            }
        }

        stage('Docker Image Build') { // Activity 4
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ."
                sh "docker tag ${DOCKER_IMAGE}:${env.BUILD_NUMBER} ${DOCKER_IMAGE}:latest"
            }
        }

        stage('Docker Image Push') { // Activity 5
            steps {
                
                withCredentials([usernamePassword(credentialsId: 'docker_hub_creds_id', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}:${env.BUILD_NUMBER}"
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }
    }
}