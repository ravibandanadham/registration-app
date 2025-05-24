pipeline {
    agent { label 'jenkins-agent' }

    tools {
        jdk 'java17'       // Jenkins Global Tool Configuration
        maven 'maven3'     // Jenkins Global Tool Configuration
    }

    environment {
        SONARQUBE_ENV = 'sonar-server'   // Must match SonarQube installation name in Jenkins
        APP_NAME= "register-app-pipeline"
        RELEASE= "1.0.0"
        DOCKER_USER= "dockerjrk"
        DOCKER_PASS= "docker-hub-cred"
        IMAGE_NAME= "${DOCKER_USER}" + "/" + "${APP_NAME}" 
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}" 
        
        
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/ravibandanadham/registration-app.git'
            }
        }

        stage('Build Application') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${env.SONARQUBE_ENV}") {
                    sh '''
                        mvn sonar:sonar \
                        -Dsonar.projectKey=registration-app-ci \
                        -Dsonar.projectName=registration-app-ci
                    '''
                }
            
            }
        }

           stage('Build Docker Image') {
            steps {
                script {
                    // Build docker image with the tag RELEASE-BUILD_NUMBER
                    dockerImage = docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }
    stage('Login and Push Docker Image') {
            steps {
                script {
                    // Authenticate with Docker Hub using Jenkins credentials
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_PASS}") {
                        dockerImage.push("${IMAGE_TAG}")  // Push with version tag
                        dockerImage.push('latest')         // Also push 'latest' tag
                    }
                }
            }
        }
    }
}
