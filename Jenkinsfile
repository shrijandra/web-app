pipeline {
    agent any
   // tools {
   //     maven "Maven3"
  //  }
 //   environment {
 //       PATH = "/usr/bin:$PATH"
 //  }   
    stages {
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
              }
        }
        stage("clone code"){
            steps{
                git branch: 'shrijandra/web-app', credentialsId: 'gitid', url: 'https://github.com/shrijandra/web-app.git'
            }
        }

        stage("Build Application"){
            steps {
                sh "mvn clean package"
            }
        }
        stage('Unit Tests - JUnit and Jacoco'){
           steps {
                sh "mvn test"
          }
        }
        stage("Sonarqube Analysis"){
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins-sonar-token') {
                    sh "mvn sonar:sonar"
                    }
                }
            }
        }
        
    } 
}      
//refddfdsfds
