// CI/CD pipeline
pipeline {
    agent any

    stages {

        stage('Build & Test') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t chandrakantyetre/college-mis:${BUILD_NUMBER} .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-college-mis',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_TOKEN'
                )]) {
                    sh '''
                        echo "$DOCKER_TOKEN" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push chandrakantyetre/college-mis:${BUILD_NUMBER}
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker pull chandrakantyetre/college-mis:${BUILD_NUMBER}

                    docker stop college-mis || true
                    docker rm college-mis || true

                    docker run -d \
                        --name college-mis \
                        -p 8081:8080 \
                        chandrakantyetre/college-mis:${BUILD_NUMBER}
                '''
            }
        }
    }
}
