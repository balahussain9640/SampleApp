pipeline {
    agent any
    tools {
        maven 'Maven' // Name of the Maven installation configured in Jenkins
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/balahussain9640/SampleApp']]])
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
    }

    post {
        success {
            echo 'Build Completed Successfully!'
        }
        failure {
            echo 'Build Failed. Check Logs!'
        }
    }
}
