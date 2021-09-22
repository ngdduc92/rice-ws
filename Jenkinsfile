pipeline {
    environment {
        VERSION = sh(returnStdout:true, script: 'mvn exec:exec -Dexec.executable=awk -Dexec.args="\'BEGIN {printf(\\"%s-\\", gensub(/-SNAPSHOT/,\\"\\", \\"g\\", \\"\\${project.version}\\"));system(\\"git rev-parse --short=5 HEAD\\")}\'" --non-recursive -q').trim()
        MODULE_NAME = "rice-ws"
    }
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Build and Unit testing') {
            steps {
                withMaven() {
                    sh 'mvn clean package -U'
                }
            }
        }
        stage('Build and push Docker image to Docker Hub') {
            steps {
                sh "docker build -t ngdduc92/rice-ws-registry/${MODULE_NAME}:${VERSION} ."
                withCredentials([string(credentialsId: 'docker-hub-token', variable: 'docker-hub-pwd')]) {
                    sh "docker login -u ngdduc92 -p ${docker-hub-pwd}"
                }
                sh "docker push ngdduc92/rice-ws-registry/${MODULE_NAME}:${VERSION}"
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
    post {
        always {
            junit '**/target/surefire-reports/*.xml'
            echo 'Deleting workspace...'
            deleteDir()
            echo 'Workspace deleted.'
        }
    }
}
