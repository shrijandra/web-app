pipeline {
    agent any
   // tools {
   //     maven "Maven3"
  //  }
 //   environment {
 //       PATH = "/usr/bin:$PATH"
 //  }  

    environment {
	    APP_NAME = "register-app-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "shrijandra"
        DOCKER_PASS = 'dockerpsw'
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	    JENKINS_API_TOKEN = credentials("JENKINS_API_TOKEN")
    } 
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
        stage("Quality Gate"){
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonar-token'
                }
            }
        }
        stage("Build and push docker image"){
            steps {
                script {
                    docker.withDockerRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }
                    docker.withDockerRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_NAME}")
                        docker_image.push('latest')
                    }
                }
            }
        }
        
    } 
}      
//refddfdsfds
