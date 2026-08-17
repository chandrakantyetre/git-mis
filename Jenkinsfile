stage('Docker CLI Login Test') {
    steps {
        withEnv([
            'DOCKER_HOST=npipe:////./pipe/dockerDesktopLinuxEngine',
            'DOCKER_CONFIG=%WORKSPACE%\\.docker-login-test'
        ]) {
            withCredentials([usernamePassword(
                credentialsId: 'dockerhub-college-mis',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_TOKEN'
            )]) {
                bat '''
                    if exist "%DOCKER_CONFIG%" rmdir /s /q "%DOCKER_CONFIG%"
                    mkdir "%DOCKER_CONFIG%"

                    echo %DOCKER_TOKEN% | docker login -u "%DOCKER_USER%" --password-stdin
                    if errorlevel 1 exit /b 1

                    echo Docker CLI login SUCCESS
                '''
            }
        }
    }
}