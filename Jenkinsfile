pipeline {
    agent any
    tools {
        maven 'MAVEN_HOME' // Name of the Maven installation configured in Jenkins
    }

    parameters {
    string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build')
}
    stages {
        stage('Checkout') {
        steps {
            git branch: params.BRANCH_NAME, url: 'https://github.com/balahussain9640/SampleApp'
        }
        // stage('Checkout') {
        //     steps {
        //         checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/balahussain9640/SampleApp']]])
        //     }
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
    always {
        junit 'target/surefire-reports/*.xml'
    }
    failure {
        echo 'Build Failed. Check Logs!'
    }
 }

}
