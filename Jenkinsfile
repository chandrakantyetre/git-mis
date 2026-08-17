pipeline {
    agent any

    stages {

        stage('Docker Endpoint Test') {
            steps {
                withEnv(['DOCKER_HOST=npipe:////./pipe/dockerDesktopLinuxEngine']) {
                    bat '''
                        echo ===== WINDOWS USER =====
                        whoami

                        echo ===== DOCKER HOST =====
                        echo %DOCKER_HOST%

                        echo ===== DOCKER VERSION =====
                        docker version
                    '''
                }
            }
        }
    }
}