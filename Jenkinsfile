pipeline {
    agent any

    stages {

        stage('Docker Hub Auth Test') {
            steps {
                withEnv(['DOCKER_HOST=npipe:////./pipe/dockerDesktopLinuxEngine']) {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockerhub-college-mis',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_TOKEN'
                    )]) {
                        powershell '''
                            $body = @{
                                identifier = $env:DOCKER_USER
                                secret     = $env:DOCKER_TOKEN
                            } | ConvertTo-Json

                            try {
                                $response = Invoke-RestMethod `
                                    -Uri "https://hub.docker.com/v2/auth/token" `
                                    -Method Post `
                                    -ContentType "application/json" `
                                    -Body $body

                                if ($response.access_token) {
                                    Write-Host "Docker Hub API authentication: SUCCESS"
                                } else {
                                    Write-Host "Docker Hub API authentication: FAILED - no access token"
                                    exit 1
                                }
                            }
                            catch {
                                Write-Host "Docker Hub API authentication: FAILED"
                                Write-Host $_.Exception.Message
                                exit 1
                            }
                        '''
                    }
                }
            }
        }
    }
}