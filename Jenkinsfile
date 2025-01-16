pipeline {
    agent any
    tools {
        maven 'MAVEN_HOME' // Name of the Maven installation configured in Jenkins
    }

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build')
        string(name: 'IMAGE_NAME', defaultValue: 'bala_sampleapp', description: 'Docker image name') // Docker image name with "bala"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: params.BRANCH_NAME, url: 'https://github.com/balahussain9640/SampleApp'
            }
        }

        stage('Build') {
            steps {
                // Run Maven build
                bat 'mvn clean install'
            }
        }

        stage('Test') {
            steps {
                // Run Maven tests
                bat 'mvn test'
            }
        }

        stage('Package') {
            steps {
                // Package the application
                bat 'mvn package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image with the specified name
                    bat "docker build -t ${params.IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove the existing container if it exists
                    bat '''
                    docker ps -a -q --filter "name=bala_sampleapp_container" | findstr . && docker rm -f bala_sampleapp_container || echo "No existing container to remove"
                    '''
            
                    // Run a new container
                    bat "docker run -d --name bala_sampleapp_container -p 8081:8080 ${params.IMAGE_NAME}:latest"
                }
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
        failure {
            echo 'Build Failed. Check Logs!'
        }
    }
}
