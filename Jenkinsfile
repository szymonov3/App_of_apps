def frontendImage="szymonov/frontend"
def backendImage="szymonov/backend"
def backendDockerTag=""
def frontendDockerTag=""
def dockerRegistry=""
def registryCredentials="dockerhub"
pipeline {
    agent {
        label 'agent'
    } 

    stages {
        stage('Get Code') {
            steps {
                checkout scm
            }
        }
    }
}
