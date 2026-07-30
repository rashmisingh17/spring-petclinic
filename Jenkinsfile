pipeline {
    agent any
    environment {
        DOCKER_USER = 'your-dockerhub-username' 
        IMAGE = "payment-gateway:${BUILD_NUMBER}"
    }
    stages {
        stage('1. Build (Maven)') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('2. Docker Build & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "docker build -t ${DOCKER_USER}/${IMAGE} ."
                    sh "echo \$PASS | docker login -u \$USER --password-stdin"
                    sh "docker push ${DOCKER_USER}/${IMAGE}"
                }
            }
        }
    }
}
