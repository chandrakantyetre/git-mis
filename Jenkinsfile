pipeline {
    agent any

    stages {
        stage('Docker Environment Test') {
            steps {
                bat '''
                    echo ===== WINDOWS USER =====
                    whoami

                    echo ===== DOCKER CONTEXTS =====
                    docker context ls

                    echo ===== CURRENT CONTEXT =====
                    docker context show

                    echo ===== DOCKER VERSION =====
                    docker version
                '''
            }
        }
    }
}