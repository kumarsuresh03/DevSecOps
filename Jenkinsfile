pipeline {
    agent any // Use any available agent
    
    environment {
        SONAR_TOKEN = credentials('sonarcloud-token') // Replace with your SonarCloud token credentials ID
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out code from SCM"
                checkout scm // Use the SCM defined in Jenkins
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv(credentialsId: 'sonarcloud-token', installationName: 'SonarCloud') {
                    sh '''
                        sonar-scanner \
                        -Dsonar.projectKey=kumarsuresh03_CA3 \
                        -Dsonar.organization=kumarsuresh03 \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=https://sonarcloud.io
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning workspace after the pipeline runs"
            cleanWs() // Clean workspace after the pipeline runs
        }
    }
}
