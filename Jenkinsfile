pipeline {
    agent any
    environment{
        PATH = "/opt/apache-maven-3.9.2/bin:$PATH"
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
            }
        }
        stage('Build Docker Image'){
            steps{
                sh "docker build . -t nk95/validation-service:${DOCKER_TAG}"
            }
        }
        stage('Docker Hub Push'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u nk95 -p ${dockerHubPwd}"
                    sh "docker push nk95/validation-service:${DOCKER_TAG}"
                }
            }
        }
    }
}

def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
}