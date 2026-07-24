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
                        @echo off

                        echo %DOCKERHUB_PASSWORD%|docker login -u %DOCKERHUB_USERNAME% --password-stdin
                        if errorlevel 1 exit /b 1

                        docker tag malinthamaduranga/2026gscomp291p2:backend malinthamaduranga/2026gscomp291p2:backend-%BUILD_NUMBER%
                        if errorlevel 1 exit /b 1

                        docker tag malinthamaduranga/2026gscomp291p2:frontend malinthamaduranga/2026gscomp291p2:frontend-%BUILD_NUMBER%
                        if errorlevel 1 exit /b 1

                        docker push malinthamaduranga/2026gscomp291p2:backend
                        if errorlevel 1 exit /b 1

                        docker push malinthamaduranga/2026gscomp291p2:backend-%BUILD_NUMBER%
                        if errorlevel 1 exit /b 1

                        docker push malinthamaduranga/2026gscomp291p2:frontend
                        if errorlevel 1 exit /b 1

                        docker push malinthamaduranga/2026gscomp291p2:frontend-%BUILD_NUMBER%
                        if errorlevel 1 exit /b 1
                    '''
                }
            }
        }
    }
}
