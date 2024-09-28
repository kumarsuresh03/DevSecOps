pipeline {
    agent any // Use any available agent
    
    environment {
        SONAR_TOKEN = credentials('sonarcloud-token') // Replace with your SonarCloud token credentials ID
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm // Use the SCM defined in Jenkins
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarCloud') { // Replace 'SonarCloud' with your SonarQube server configuration name
                        sh '''
                        sonar-scanner \
                            -Dsonar.projectKey=kumarsuresh03_CA3 \  
                            -Dsonar.organization=kumarsuresh03 \ 
                            -Dsonar.sources=. \
                            -Dsonar.host.url=https://sonarcloud.io \
                            -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs() // Clean workspace after the pipeline runs
        }
    }
}
