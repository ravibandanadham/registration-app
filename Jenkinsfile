pipeline {
    agent { label 'jenkins-agent' }

    tools {
        jdk 'java17'    // Tool name from Jenkins global config
        maven 'maven3'  // Tool name from Jenkins global config
    }

    stages {
        stage('Clean up the workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Check out from SCM') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/ravibandanadham/registration-app.git'
            }
        }

        stage('Build the application') {
            steps {
                sh "mvn clean package"
            }
        }

        stage('Perform unit tests with Maven') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-scanner') {  // Replace with your actual server name in Jenkins config
                    withCredentials([string(credentialsId: 'sonar', variable: 'SONAR_TOKEN')]) {
                        sh '''
                            mvn sonar:sonar \
                            -Dsonar.projectKey=$JOB_NAME \
                            -Dsonar.projectName=$JOB_NAME \
                            -Dsonar.host.url=http://172.31.10.124:9000 \
                            -Dsonar.token=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }

        stage('Sonar Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
