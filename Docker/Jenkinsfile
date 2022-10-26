pipeline {
    agent any

    stages {
        stage('git checkout') {
            steps {
               checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/nikhil60961/python-cicd.git']]])
            }
            
        }
        stage('docker build') {
              steps{
                script {
                dir('/var/lib/jenkins/workspace/python-cicd/Docker/'){
                app = docker.build("gcp-test-366209/python:${env.BUILD_ID}")
                 }
                 }
               } 
        } 
        stage('deploy'){
              steps{
                script{
                  docker.withRegistry('https://gcr.io', 'gcr:gcp-test') {
                  app.push("${env.BUILD_NUMBER}")
                  app.push("latest")
                 }
               }
            }
        }
                
            }
        
}
