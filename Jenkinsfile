pipeline {
    agent any

    environment {
        // Environment variables
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials')
        SONAR_TOKEN = credentials('sonarcloud-token')
        DOCKER_IMAGE = 'yourdockerhubusername/simple-node-app'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/yourusername/your-repo.git'
            }
        }

        stage('Static Code Analysis - SonarCloud') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''
                    sonar-scanner \
                        -Dsonar.projectKey=yourprojectkey \
                        -Dsonar.organization=yourorganization \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }

        stage('Security Scan - Trivy') {
            steps {
                sh 'trivy fs --exit-code 1 .'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'DOCKERHUB_CREDENTIALS') {
                        sh 'docker push $DOCKER_IMAGE:latest'
                    }
                }
            }
        }
    }

    post {
        always {
            node {
                cleanWs()
            }
        }
    }
}
