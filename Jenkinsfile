pipeline {
    environment {
        VERSION = '0.0.1'
        MODULE_NAME = 'rice-ws'
        DOCKER_HUB_USERID = 'ngdduc92'
        DOCKER_HUB_PWD = credentials('docker-hub-token')
    }
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Build and Unit testing') {
            options {
                timeout(time: 3, unit: "MINUTES")
            }
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build and push Docker image to Docker Hub') {
            options {
                timeout(time: 3, unit: "MINUTES")
            }
            steps {
                sh "docker build -t ${DOCKER_HUB_USERID}/${MODULE_NAME}:latest ."
                sh "docker tag ${DOCKER_HUB_USERID}/${MODULE_NAME}:latest ${DOCKER_HUB_USERID}/${MODULE_NAME}:${VERSION}"
                sh "docker login -u ${DOCKER_HUB_USERID} -p ${DOCKER_HUB_PWD}"
                sh "docker push ${DOCKER_HUB_USERID}/${MODULE_NAME}:latest"
                sh "docker push ${DOCKER_HUB_USERID}/${MODULE_NAME}:${VERSION}"
            }
        }
        stage('Integration testing') {
            options {
                timeout(time: 3, unit: "MINUTES")
            }
            steps {
                echo 'Integration Testing '
            }
        }
        stage('Release') {
            options {
                timeout(time: 3, unit: "MINUTES")
            }
            steps {
                echo 'Releasing'
            }
        }
    }
    post {
        always {
            sh 'docker rmi -f $(docker images -q)'
            cleanWs()
        }
    }
}
