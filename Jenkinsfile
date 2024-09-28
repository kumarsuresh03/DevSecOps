pipeline {
    agent any // Use any available agent
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        SONAR_TOKEN = credentials('sonarcloud-token')
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
                    withSonarQubeEnv('SonarCloud') {
                        sh '''
                        sonar-scanner \
                            -Dsonar.projectKey=your_project_key \
                            -Dsonar.organization=your_organization \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=https://sonarcloud.io \
                            -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }

        stage('Docker Build and Push') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        // Docker login
                        sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'

                        // Build and push Docker image
                        sh '''
                        docker build -t yourdockerhubusername/devsecops:latest .
                        docker push yourdockerhubusername/devsecops:latest
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
