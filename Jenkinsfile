pipeline {
    agent  {
	docker {
      image 'abhishekf5/maven-abhishek-docker-agent:v1'
      args '--user root -v /var/run/docker.sock:/var/run/docker.sock' // mount Docker socket to access the host's Docker daemon
    }
}

    environment {
        // Define environment variables
        //#DOCKER_HUB_USERNAME = credentials('docker-hub-username')
        //#DOCKER_HUB_PASSWORD = credentials('docker-hub-password')
		DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials')
        DOCKER_IMAGE_NAME = 'springboot-helloworld'
        DOCKER_IMAGE_TAG = "${env.Build_Number}"
    }

    stages {
	stage('Maven Build') {
            steps {
                // Use Maven to build the Spring Boot application
                sh 'mvn clean package'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}")
                }
            }
        }
        
        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
				docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_CREDENTIALS) {
                        docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}").push()
                    // Log in to Docker Hub
                    //#docker.withRegistry('https://index.docker.io/v1/', DOCKER_HUB_USERNAME, DOCKER_HUB_PASSWORD) {
                        // Push the Docker image to Docker Hub
                        //#docker.image("${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
