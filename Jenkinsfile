pipeline {
    agent any

    stages {
        stage('Checkout Repository') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                bat 'docker compose -f docker-compose.yaml build'
            }
        }

        stage('Push Docker Images') {
            steps {
                withCredentials([
                        usernamePassword(
                                credentialsId: 'dockerhub-credentials',
                                usernameVariable: 'DOCKERHUB_USERNAME',
                                passwordVariable: 'DOCKERHUB_PASSWORD'
                        )
                ]) {
                    bat '''
                        echo %DOCKERHUB_PASSWORD% | docker login -u %DOCKERHUB_USERNAME% --password-stdin

                     docker tag 2026gscomp291-backend:latest %DOCKERHUB_USERNAME%/2026gscomp291-backend:%BUILD_NUMBER%
docker tag 2026gscomp291-backend:latest %DOCKERHUB_USERNAME%/2026gscomp291:backend-%BUILD_NUMBER%
docker tag 2026gscomp291-backend:latest %DOCKERHUB_USERNAME%/2026gscomp291:backend-latest

docker tag 2026gscomp291-frontend:latest %DOCKERHUB_USERNAME%/2026gscomp291:frontend-%BUILD_NUMBER%
docker tag 2026gscomp291-frontend:latest %DOCKERHUB_USERNAME%/2026gscomp291:frontend-latest

docker push %DOCKERHUB_USERNAME%/2026gscomp291:backend-%BUILD_NUMBER%
docker push %DOCKERHUB_USERNAME%/2026gscomp291:backend-latest

docker push %DOCKERHUB_USERNAME%/2026gscomp291:frontend-%BUILD_NUMBER%
docker push %DOCKERHUB_USERNAME%/2026gscomp291:frontend-latest
                    '''
                }
            }
        }



    }
}
