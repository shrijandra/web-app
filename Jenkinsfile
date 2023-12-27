pipeline {
    agent any
    tools {
        maven "Maven3"
    }
    environment {
        PATH = "/usr/bin:$PATH"
    }   
    stages {
        stage("clone code"){
            steps{
                git branch: 'shrijandra/web-app', credentialsId: 'gitid', url: 'https://github.com/shrijandra/web-app.git'
            }
        }
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
              }
        }
//added a line
      //stage("Checkout from SCM")
      //      steps {
      //          git branch: 'main', credentialsId: 
      //      }

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
    } 
}      
//refddfdsfds
