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
    tools {
        terraform 'Terraform'
    }
    parameters {
        string(name: 'backendDockerTag', defaultValue: 'latest', description: 'Tag for the backend Docker image')
        string(name: 'frontendDockerTag', defaultValue: 'latest', description: 'Tag for the frontend Docker image')
    }
    environment {
        PIP_BREAK_SYSTEM_PACKAGES = "1"
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
        
        
        stage('Clean running containers') {
            steps {
                sh "docker rm -f frontend backend"
            }
        }

        
        stage('Deploy application') {
            steps {
                script {
                    withEnv(["FRONTEND_IMAGE=$frontendImage:$frontendDockerTag", 
                             "BACKEND_IMAGE=$backendImage:$backendDockerTag"]) {
                        docker.withRegistry("$dockerRegistry", "$registryCredentials") {
                            sh "docker-compose up -d"
                        }
                    }
                }
            }
        }
        stage('Run Selenium Tests') {
            steps {
                script {
                    sh "pip3 install -r test/selenium/requirements.txt"
                    sh "python3 -m pytest test/selenium/frontendTest.py"
                }
            }
        }
        stage('Run terraform') {
            steps {
                dir('Terraform') {                
                    git branch: 'main', url: 'https://github.com/Panda-Academy-Core-2-0/Terraform'
                    withAWS(credentials:'AWS', region: 'us-east-1') {
                            sh 'terraform init -backend-config=bucket=panda-academy-panda-devops-core-n'
                            sh 'terraform apply -auto-approve -var bucket_name=panda-academy-panda-devops-core-n'
                            
                    } 
                }
            }
        }
    }
    post {
        always {
            script {
                sh "docker rm -f frontend"
                sh "docker rm -f backend"
                sh "docker-compose down"
                cleanWs()
            }
        }
    }
}

