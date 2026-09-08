// CI/CD pipeline for auto deloyed with webhook
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
            NEW_IMAGE="chandrakantyetre/college-mis:${BUILD_NUMBER}"
            LAST_GOOD_FILE="/var/lib/jenkins/last_successful_image"

            LAST_GOOD_IMAGE=$(cat "$LAST_GOOD_FILE")

            echo "New image: $NEW_IMAGE"
            echo "Last known good image: $LAST_GOOD_IMAGE"

            docker pull "$NEW_IMAGE"

            docker stop college-mis || true
            docker rm college-mis || true

            docker run -d \
    --name college-mis \
    --restart unless-stopped \
    -p 8081:8080 \
    "$NEW_IMAGE"

            echo "Waiting for application to start..."
            sleep 10

            if curl -f http://localhost:8081/actuator/health; then

                echo "Health check PASSED"
                echo "Deployment successful: $NEW_IMAGE"

                echo "$NEW_IMAGE" > "$LAST_GOOD_FILE"

                echo "Last known good image updated to: $NEW_IMAGE"

            else

                echo "Health check FAILED"
                echo "Rolling back to: $LAST_GOOD_IMAGE"

                docker stop college-mis || true
                docker rm college-mis || true

                docker run -d \
    --name college-mis \
    --restart unless-stopped \
    -p 8081:8080 \
    "$LAST_GOOD_IMAGE"

                echo "Rollback completed"
                exit 1
            fi
        '''
    }
}
    }
}
