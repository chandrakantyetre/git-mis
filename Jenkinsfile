pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                bat "docker build -t chandrakantyetre/college-mis:%BUILD_NUMBER% ."
            }
        }

        stage('Docker Credential Test') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-college-mis',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    powershell '''
                        $bytes = [System.Text.Encoding]::UTF8.GetBytes($env:DOCKER_TOKEN)
                        $hash = [System.Security.Cryptography.SHA256]::Create().ComputeHash($bytes)
                        $hashText = ([BitConverter]::ToString($hash)).Replace("-", "").ToLower()

                        Write-Host "Docker username: $env:DOCKER_USER"
                        Write-Host "PAT HASH: $hashText"
                    '''
                }
            }
        }
    }
}