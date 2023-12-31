pipeline {
    agent any
    tools {
       maven "Maven3"
    }
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
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build("${IMAGE_NAME}")
                    }
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }

        stage ('Cleanup Artifacts') {
           steps {
               script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
               }
          }
       }

        stage("Trigger CD Pipeline") {
            steps {
                script {
                    sh "curl -v -k --user jenkins:${JENKINS_API_TOKEN} -X POST -H 'cache-control: no-cache' -H 'content-type: application/x-www-form-urlencoded' --data 'IMAGE_TAG=${IMAGE_TAG}' 'ec2-18-224-150-91.us-east-2.compute.amazonaws.com:8080/job/git_ops_web_app/buildWithParameters?token=gitops-token'"
                }
            }
       }
    }
 } 
    
//refddfdsfds
