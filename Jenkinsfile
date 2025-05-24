//defining the declarative pipeline script 
pipeline {
  agent { label 'jenkins-agent' }
  tools{
    //ddefining java and maven tools for integration & build 
    jdk 'java17' \\ tool name configured in the jenkins global tools 
    maven 'maven3' \\ tool name configured in the jenkins global tools 
  }
  \\ defining cleaning up the work space 
  stages {
     stage ('clean up the workspace ')
    {
      steps {
         cleanWs()
      }
    }
  stage ('check out from the scm') 
    steps {
      git branch: 'main', credentialsId: 'github', url: 'https://github.com/ravibandanadham/registration-app.git'
    }
    stage ('Build the application '){
    steps {
      sh "mvn clean package"  \\generating war or var file 
    }
    }
 stage ('perform the unit test with maven')  {
    steps {
      sh 'mvn test' \\ maven golad to test application 
    }
 }
    
}
