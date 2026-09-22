// Student Task Manager - Jenkins Pipeline
// This pipeline demonstrates CI/CD for a simple web application

pipeline {
    // Agent can be 'any' or specify a label
    agent any

    // Define environment variables if needed
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

                // Clone the repository (shallow clone for speed)
                checkout scm

                // List the files to verify checkout
                echo 'Files fetched:'
                sh 'ls -la'
            }
        }

        // Stage 2: Validate Application - Check that required files exist
        stage('Validate Application') {
            steps {
                echo 'Stage 2: Validating application files...'

                // Check if index.html exists
                echo 'Checking for index.html...'
                sh 'test -f index.html && echo "index.html found" || exit 1'

                // Check if style.css exists
                echo 'Checking for style.css...'
                sh 'test -f style.css && echo "style.css found" || exit 1'

                // Check if script.js exists
                echo 'Checking for script.js...'
                sh 'test -f script.js && echo "script.js found" || exit 1'

                // Verify file size is not empty
                sh 'test -s index.html && test -s style.css && test -s script.js'

                echo 'All required files validated successfully!'
            }
        }

        // Stage 3: Build Docker Image
        stage('Build Docker Image') {
            steps {
                echo 'Stage 3: Building Docker image...'

                // Remove existing image if it exists (ignore errors)
                sh "docker rmi ${DOCKER_IMAGE} 2>/dev/null || true"

                // Build the Docker image
                // -t tags the image with the name
                // . indicates the build context (current directory)
                sh "docker build -t ${DOCKER_IMAGE} ."

                // Verify the image was created
                sh "docker images ${DOCKER_IMAGE}"

                echo 'Docker image built successfully!'
            }
        }

        // Stage 4: Deploy Container
        stage('Deploy Container') {
            steps {
                echo 'Stage 4: Deploying Docker container...'

                // Stop and remove existing container if it exists
                // Using || true to prevent pipeline failure if container doesn't exist
                // Note: \${APP_NAME} escapes the first $ for shell; ${APP_NAME} is Groovy interpolation
                sh """
                    if docker ps -a --format '{{.Names}}' | grep -q '^\${APP_NAME}\$'; then
                        echo 'Stopping existing container...'
                        docker stop ${APP_NAME} || true
                        echo 'Removing existing container...'
                        docker rm ${APP_NAME} || true
                    else
                        echo 'No existing container found, creating new one...'
                    fi
                """

                // Run the Docker container
                // -d: detached mode (runs in background)
                // -p: port mapping (host:container)
                // --name: name the container
                sh "docker run -d -p ${CONTAINER_PORT}:${NGINX_PORT} --name ${APP_NAME} ${DOCKER_IMAGE}"

                // Wait a moment for container to start
                sleep 5

                // Verify container is running
                sh "docker ps --filter 'name=${APP_NAME}'"

                echo 'Container deployed successfully!'
                echo "Application available at: http://localhost:${CONTAINER_PORT}"
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
            echo '✓ Build and deployment successful!'
        }

        // Run only if the build fails
        failure {
            echo '✗ Build or deployment failed. Check logs above.'
        }
    }
}