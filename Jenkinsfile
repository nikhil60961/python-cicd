pipeline {
    agent any
    	environment {
		
		            PROJECT_ID = 'gcp-test-366209'
                CLUSTER_NAME = 'automation'
                LOCATION = 'us-east1'
                CREDENTIALS_ID = 'gcp-test'		
	}

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
        stage('Deployment to GKE') {
            steps{
                sh "sed -i 's/latest:${env.BUILD_ID}/g' deployment.yaml"
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }
            }
        
}
sed -i 's/python:latest/python:80/g' /var/lib/jenkins/workspace/python-cicd/deployment.yaml