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
	string 'backendDockerTag',
        string 'frontendDockerTag'
    }
    stages {
        stage('Get Code') {
            steps {
                checkout scm
            }
        }
    }

    stages {
        stage('Adjust version') {
            steps {
                script{
                    backendDockerTag = params.backendDockerTag.isEmpty() ? "latest" : params.backendDockerTag
                    frontendDockerTag = params.frontendDockerTag.isEmpty() ? "latest" : params.frontendDockerTag
                    
                    currentBuild.description = "Backend: ${backendDockerTag}, Frontend: ${frontendDockerTag}"
                }
            }
        }
    }
}
