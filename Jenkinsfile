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
    parameters {
        string(name: 'backendDockerTag', defaultValue: '', description: 'Tag for the backend Docker image')
        string(name: 'frontendDockerTag', defaultValue: '', description: 'Tag for the frontend Docker image')
    }
    stages {
        stage('Get Code') {
            steps {
                checkout scm
            }
        }
        stage('Adjust version') {
            steps {
                script {
                    backendDockerTag = params.backendDockerTag.isEmpty() ? "latest" : params.backendDockerTag
                    frontendDockerTag = params.frontendDockerTag.isEmpty() ? "latest" : params.frontendDockerTag

                    currentBuild.description = "Backend: ${backendDockerTag}, Frontend: ${frontendDockerTag}"
                }
            }
        }
    }
    post {
        always {
            script {
                sh "docker rm -f frontend"
            }
        }
    }
}

