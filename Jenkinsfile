pipeline {
    agent any

    stages {

        stage('Docker Endpoint Test') {
            steps {
                bat '''
                    echo ===== WINDOWS USER =====
                    whoami

                    echo ===== DOCKER VERSION USING DESKTOP LINUX PIPE =====
                    docker -H npipe:////./pipe/dockerDesktopLinuxEngine version

                    echo ===== DOCKER INFO USING DESKTOP LINUX PIPE =====
                    docker -H npipe:////./pipe/dockerDesktopLinuxEngine info
                '''
            }
        }
    }
}