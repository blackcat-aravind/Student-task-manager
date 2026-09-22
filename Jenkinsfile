// Student Task Manager - Jenkins Pipeline
// This pipeline demonstrates CI/CD for a simple web application
// Designed for Windows Jenkins environment

pipeline {
    // Agent can be 'any' or specify a label
    agent any

    // Define environment variables for the application
    environment {
        APP_NAME = 'student-task-manager'
        DOCKER_IMAGE = 'student-task-manager'
        CONTAINER_PORT = '8080'
        NGINX_PORT = '80'
    }

    stages {
        // Stage 1: Checkout - Get the code from GitHub
        stage('Checkout') {
            steps {
                // Display a message to the user
                echo 'Stage 1: Checking out code from GitHub...'

                // Clone the repository using Jenkins SCM
                checkout scm

                // List the files to verify checkout (Windows command)
                echo 'Files fetched:'
                bat 'dir /b'
            }
        }

        // Stage 2: Validate Application - Check that required files exist
        stage('Validate Application') {
            steps {
                echo 'Stage 2: Validating application files...'

                // Check if index.html exists using Windows IF EXIST
                echo 'Checking for index.html...'
                bat 'if exist index.html (echo index.html found) else (exit /b 1)'

                // Check if style.css exists
                echo 'Checking for style.css...'
                bat 'if exist style.css (echo style.css found) else (exit /b 1)'

                // Check if script.js exists
                echo 'Checking for script.js...'
                bat 'if exist script.js (echo script.js found) else (exit /b 1)'

                // Verify files are not empty (Windows approach)
                bat 'for %%F in (index.html style.css script.js) do (if %%~zF==0 exit /b 1)'

                echo 'All required files validated successfully!'
            }
        }

        // Stage 3: Build Docker Image
        stage('Build Docker Image') {
            steps {
                echo 'Stage 3: Building Docker image...'

                // Remove existing image if it exists (ignore errors)
                // Windows: redirect NUL to suppress error output
                bat 'docker rmi %DOCKER_IMAGE% 2> NUL || echo No existing image to remove'

                // Build the Docker image
                // -t tags the image with the name
                // . indicates the build context (current directory)
                bat 'docker build -t %DOCKER_IMAGE% .'

                // Verify the image was created
                bat 'docker images %DOCKER_IMAGE%'

                echo 'Docker image built successfully!'
            }
        }

        // Stage 4: Deploy Container
        stage('Deploy Container') {
            steps {
                echo 'Stage 4: Deploying Docker container...'

                // Stop and remove existing container if it exists
                // Using || true equivalent to prevent pipeline failure if container doesn't exist
                // Windows batch IF EXIST checks for container, docker stop/rm with || true
                bat '''
                docker ps -a --format "{{.Names}}" | findstr /C:"%APP_NAME%" > NUL
                if %ERRORLEVEL% equ 0 (
                    echo Stopping existing container...
                    docker stop %APP_NAME% > NUL 2>&1 || echo Container was not running
                    echo Removing existing container...
                    docker rm %APP_NAME% > NUL 2>&1 || echo Container removal skipped
                ) else (
                    echo No existing container found, creating new one...
                )
                '''

                // Run the Docker container
                // -d: detached mode (runs in background)
                // -p: port mapping (host:container)
                // --name: name the container
                bat 'docker run -d -p %CONTAINER_PORT%:%NGINX_PORT% --name %APP_NAME% %DOCKER_IMAGE%'

                // Wait a moment for container to start (Windows timeout)
                bat 'timeout /t 5 /nobreak > NUL'

                // Verify container is running
                bat 'docker ps --filter "name=%APP_NAME%"'

                echo 'Container deployed successfully!'
                echo "Application available at: http://localhost:%CONTAINER_PORT%"
            }
        }
    }

    // Post-build actions - runs after the pipeline finishes
    post {
        // Always run these actions regardless of success/failure
        always {
            echo 'Pipeline execution completed.'
        }

        // Run only if the build succeeds
        success {
            echo 'Build and deployment successful!'
        }

        // Run only if the build fails
        failure {
            echo 'Build or deployment failed. Check logs above.'
        }
    }
}