pipeline {
    agent any
    environment {
        PATH = "/usr/bin:$PATH"
    }
    }    
    stages{
       
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
//refddfdsfds
