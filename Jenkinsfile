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

                powershell -NoProfile -Command "$env:DOCKERHUB_PASSWORD | docker login -u $env:DOCKERHUB_USERNAME --password-stdin"
                if errorlevel 1 exit /b 1

                docker tag 2026gscomp291-backend:latest %DOCKERHUB_USERNAME%/2026gscomp291-backend:%BUILD_NUMBER%
                if errorlevel 1 exit /b 1

                docker tag 2026gscomp291-backend:latest %DOCKERHUB_USERNAME%/2026gscomp291-backend:latest
                if errorlevel 1 exit /b 1

                docker tag 2026gscomp291-frontend:latest %DOCKERHUB_USERNAME%/2026gscomp291-frontend:%BUILD_NUMBER%
                if errorlevel 1 exit /b 1

                docker tag 2026gscomp291-frontend:latest %DOCKERHUB_USERNAME%/2026gscomp291-frontend:latest
                if errorlevel 1 exit /b 1

                docker push %DOCKERHUB_USERNAME%/2026gscomp291-backend:%BUILD_NUMBER%
                if errorlevel 1 exit /b 1

                docker push %DOCKERHUB_USERNAME%/2026gscomp291-backend:latest
                if errorlevel 1 exit /b 1

                docker push %DOCKERHUB_USERNAME%/2026gscomp291-frontend:%BUILD_NUMBER%
                if errorlevel 1 exit /b 1

                docker push %DOCKERHUB_USERNAME%/2026gscomp291-frontend:latest
                if errorlevel 1 exit /b 1
            '''
        }
    }
}