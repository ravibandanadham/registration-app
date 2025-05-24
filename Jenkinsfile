// defining the declarative pipeline script 
pipeline {
    agent { label 'jenkins-agent' }

    tools {
        // defining java and maven tools for integration & build 
        jdk 'java17'    // tool name configured in the jenkins global tools 
        maven 'maven3'  // tool name configured in the jenkins global tools 
    }

    // defining cleaning up the workspace 
    stages {
        stage('clean up the workspace') {
            steps {
                cleanWs()
            }
        }

        stage('check out from the scm') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/ravibandanadham/registration-app.git'
            }
        }

        stage('Build the application') {
            steps {
                sh "mvn clean package"  // generating war or jar file 
            }
        }

        stage('perform the unit test with maven') {
            steps {
                sh 'mvn test'  // maven goal to test application 
            }
        }
    }
}
