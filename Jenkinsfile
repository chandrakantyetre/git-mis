pipeline {
    agent any

    stages {

        stage('Build & Test') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                withEnv(['DOCKER_HOST=npipe:////./pipe/dockerDesktopLinuxEngine']) {
                    bat "docker build -t chandrakantyetre/college-mis:%BUILD_NUMBER% ."
                }
            }
        }

        stage('Docker Push') {
            steps {
                withEnv(['DOCKER_HOST=npipe:////./pipe/dockerDesktopLinuxEngine']) {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockerhub-college-mis',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_TOKEN'
                    )]) {
                        bat '''
                            echo %DOCKER_TOKEN% | docker login -u "%DOCKER_USER%" --password-stdin
                            if errorlevel 1 exit /b 1

                            docker push chandrakantyetre/college-mis:%BUILD_NUMBER%
                            if errorlevel 1 exit /b 1
                        '''
                    }
                }
            }
        }
    }
}