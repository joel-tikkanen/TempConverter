pipeline {
    agent any

    tools {
        maven 'maven 3.8.1'
        jdk 'jdk 17'
    }

    stages {
        stage('Checkout') {
            steps {
                // Get code from GitHub repository
                git 'https://github.com/joel-tikkanen/TempConverter'
            }
        }

        stage('Build') {
            steps {
                // Run Maven build
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                // Run Maven tests
                sh 'mvn test'
            }
            post {
                always {
                    // Archive the test results
                    junit '**/target/surefire-reports/TEST-*.xml'
                }
            }
        }

        stage('Code Coverage') {
            steps {
                // Run Maven verify to generate code coverage report
                sh 'mvn verify'
            }
            post {
                success {
                    // Archive the code coverage report
                    archiveArtifacts 'target/site/jacoco/**'
                }
            }
        }

        stage('Package') {
            steps {
                // Package the application
                sh 'mvn package'
            }
            post {
                success {
                    // Archive the built artifacts
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace
            cleanWs()
        }
    }
}