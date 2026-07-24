pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    stages {
        stage('Checkout Repository') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'docker compose -f docker-compose.yaml build'
                    } else {
                        bat 'docker compose -f docker-compose.yaml build'
                    }
                }
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
                    script {
                        withEnv(["DOCKER_CONFIG=${pwd(tmp: true)}/docker"]) {
                            try {
                                if (isUnix()) {
                                    sh '''
                                        set +x
                                        printf '%s' "$DOCKERHUB_PASSWORD" | docker login --username "$DOCKERHUB_USERNAME" --password-stdin

                                        docker tag malinthamaduranga/2026gscomp291p3:backend "$DOCKERHUB_USERNAME/2026gscomp291p3:backend-$BUILD_NUMBER"
                                        docker tag malinthamaduranga/2026gscomp291p3:backend "$DOCKERHUB_USERNAME/2026gscomp291p3:backend-latest"
                                        docker tag malinthamaduranga/2026gscomp291p3:frontend "$DOCKERHUB_USERNAME/2026gscomp291p3:frontend-$BUILD_NUMBER"
                                        docker tag malinthamaduranga/2026gscomp291p3:frontend "$DOCKERHUB_USERNAME/2026gscomp291p3:frontend-latest"

                                        docker push "$DOCKERHUB_USERNAME/2026gscomp291p3:backend-$BUILD_NUMBER"
                                        docker push "$DOCKERHUB_USERNAME/2026gscomp291p3:backend-latest"
                                        docker push "$DOCKERHUB_USERNAME/2026gscomp291p3:frontend-$BUILD_NUMBER"
                                        docker push "$DOCKERHUB_USERNAME/2026gscomp291p3:frontend-latest"
                                    '''
                                } else {
                                    bat '''
                                        @echo off
                                        powershell -NoProfile -NonInteractive -Command "$env:DOCKERHUB_PASSWORD | docker login --username $env:DOCKERHUB_USERNAME --password-stdin"
                                        if errorlevel 1 exit /b 1

                                        docker tag malinthamaduranga/2026gscomp291p3:backend "%DOCKERHUB_USERNAME%/2026gscomp291p3:backend-%BUILD_NUMBER%"
                                        if errorlevel 1 exit /b 1
                                        docker tag malinthamaduranga/2026gscomp291p3:backend "%DOCKERHUB_USERNAME%/2026gscomp291p3:backend-latest"
                                        if errorlevel 1 exit /b 1
                                        docker tag malinthamaduranga/2026gscomp291p3:frontend "%DOCKERHUB_USERNAME%/2026gscomp291p3:frontend-%BUILD_NUMBER%"
                                        if errorlevel 1 exit /b 1
                                        docker tag malinthamaduranga/2026gscomp291p3:frontend "%DOCKERHUB_USERNAME%/2026gscomp291p3:frontend-latest"
                                        if errorlevel 1 exit /b 1

                                        docker push "%DOCKERHUB_USERNAME%/2026gscomp291p3:backend-%BUILD_NUMBER%"
                                        if errorlevel 1 exit /b 1
                                        docker push "%DOCKERHUB_USERNAME%/2026gscomp291p3:backend-latest"
                                        if errorlevel 1 exit /b 1
                                        docker push "%DOCKERHUB_USERNAME%/2026gscomp291p3:frontend-%BUILD_NUMBER%"
                                        if errorlevel 1 exit /b 1
                                        docker push "%DOCKERHUB_USERNAME%/2026gscomp291p3:frontend-latest"
                                        if errorlevel 1 exit /b 1
                                    '''
                                }
                            } finally {
                                if (isUnix()) {
                                    sh returnStatus: true, script: 'docker logout'
                                } else {
                                    bat returnStatus: true, script: '@docker logout'
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}
