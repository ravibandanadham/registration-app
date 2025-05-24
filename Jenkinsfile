pipeline {
    agent { label 'jenkins-agent' }

    tools {
        // Use Jenkins global tool configurations
        jdk 'java17'       // JDK tool name as configured in Jenkins
        maven 'maven3'     // Maven tool name as configured in Jenkins
    }

    stages {
        stage('Clean up workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/ravibandanadham/registration-app.git'
            }
        }

        stage('Build the application') {
            steps {
                sh 'mvn clean package'  // Package the application (JAR/WAR)
            }
        }

        stage('Run unit tests') {
            steps {
                sh 'mvn test'  // Run Maven tests
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') { // Make sure this name matches Jenkins global configuration
                    sh '''
                        mvn sonar:sonar \
                        -Dsonar.projectKey=$JOB_NAME \
                        -Dsonar.projectName=$JOB_NAME
                    '''
                }
            }
        }

        stage('SonarQube Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
