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

                // Build the Docker image
                // -t: tags the image with the name (student-task-manager)
                // .: uses the current directory as build context (where Dockerfile is located)
                echo 'Building Docker image from Dockerfile...'
                bat 'docker build -t %DOCKER_IMAGE% .'

                // Verify the image was created successfully
                // This command lists images and filters by name
                echo 'Verifying Docker image was created...'
                bat 'docker image ls --filter "reference=%DOCKER_IMAGE%"'

                echo 'Docker image built successfully!'
            }
        }

        // Stage 4: Deploy Container
        stage('Deploy Container') {
            steps {
                echo 'Stage 4: Deploying Docker container...'

                // Check if container already exists using Docker's native filter
                // docker ps -a lists all containers, --filter filters by name
                // We save output to a variable to check if container exists
                echo 'Checking if container already exists...'
                bat '''
                for /f "delims=" %%i in ('docker ps -a --filter "name=%APP_NAME%" -q') do set CONTAINER_ID=%%i
                if defined CONTAINER_ID (
                    echo Container %APP_NAME% exists, stopping it...
                    docker stop %APP_NAME% > NUL 2>&1
                    echo Removing existing container...
                    docker rm %APP_NAME% > NUL 2>&1
                ) else (
                    echo No existing container found, will create new one...
                )
                set "CONTAINER_ID="
                '''

                // Run the Docker container
                // -d: detached mode (runs in background)
                // -p: port mapping (host:container) - maps port 8080 on host to port 80 in container
                // --name: assigns a name to the container for easy reference
                echo 'Starting new container...'
                bat 'docker run -d -p %CONTAINER_PORT%:%NGINX_PORT% --name %APP_NAME% %DOCKER_IMAGE%'

                // Wait for container to fully start
                // Using ping to localhost as a delay (Windows-compatible)
                bat 'ping 127.0.0.1 -n 4 > NUL'

                // Verify container is actually running after startup
                echo 'Verifying container is running...'
                bat 'docker ps --filter "status=running" --filter "name=%APP_NAME%"'

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