pipeline {
    agent any
    stages {
        stage('Build and Unit testing') {
            steps {
                sh "mvn clean package"
            }
        }
        stage('Build and push Docker image to Docker Hub') {
            steps {
                sh "docker build -t ngdduc92/rice-ws:0.0.1 ."
                withCredentials([string(credentialsId: 'docker-hub-token', variable: 'docker-hub-pwd')]) {
                    sh "docker login -u ngdduc92 -p ${docker-hub-pwd}"
                }
                sh "docker push ngdduc92/rice-ws:0.0.1"
            }
        }
        stage('Integration testing') {
            steps {
                echo 'Integration Testing'
            }
        }
        stage('Release') {
            steps {
                echo 'Releasing'
            }
        }
    }
}
