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

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-college-mis',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    powershell '''
                        $env:DOCKER_TOKEN | docker login -u $env:DOCKER_USER --password-stdin

                        if ($LASTEXITCODE -ne 0) {
                            exit 1
                        }

                        docker push "chandrakantyetre/college-mis:$env:BUILD_NUMBER"

                        if ($LASTEXITCODE -ne 0) {
                            exit 1
                        }
                    '''
                }
            }
        }
    }
}