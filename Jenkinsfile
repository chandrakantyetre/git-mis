pipeline {
    agent any

    stages {

        stage('Docker Login Test') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-college-mis',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    powershell '''
                        $configDir = "$env:WORKSPACE\\.docker-jenkins"

                        if (Test-Path $configDir) {
                            Remove-Item $configDir -Recurse -Force
                        }

                        New-Item -ItemType Directory -Path $configDir | Out-Null

                        $env:DOCKER_CONFIG = $configDir

                        $env:DOCKER_TOKEN | docker login `
                            -u $env:DOCKER_USER `
                            --password-stdin

                        if ($LASTEXITCODE -ne 0) {
                            exit 1
                        }

                        Write-Host "Docker login successful from Jenkins service environment."
                    '''
                }
            }
        }
    }
}