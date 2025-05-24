pipeline {
    agent { label 'jenkins-agent' }

    tools {
        jdk 'java17'       // Jenkins Global Tool Configuration
        maven 'maven3'     // Jenkins Global Tool Configuration
    }

    environment {
        SONARQUBE_ENV = 'sonar-server'   // Must match SonarQube installation name in Jenkins
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

      
    }
}
