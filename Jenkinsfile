


pipeline {
    agent any
    environment {
        PROJECT_ID = 'qwiklabs-gcp-00-1266fe1cca53'
        CLUSTER_NAME = 'jenkins-cd'
        LOCATION = 'us-east1-d'
        CREDENTIALS_ID = 'qwiklabs-gcp-00-1266fe1cca53'
        dockerImage = "hello-world-python"
    }
    stages {
        stage("Checkout code") {
            steps {
              git branch: 'main', url: 'https://github.com/shivam779823/jenkins-project.git'  
            }
        }
        stage("Build image") {
            steps {
                script {
                    dockerImage = docker.build("shiva9921/hello-world-python:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhublogin') {
                            dockerImage.push("latest")
                            dockerImage.push("${env.BUILD_ID}")
                    }
                }
            }
        }        
        stage('Deploy to GKE') {
            steps{
                sh "sed -i 's/hello-world-python:latest/hello-world-python:${env.BUILD_ID}/g' deployment.yaml" qwiklabs-gcp-04-6967b1341be1
                step([$class: 'KubernetesEngineBuilder', projectId: env.PROJECT_ID, clusterName: env.CLUSTER_NAME, location: env.LOCATION, manifestPattern: 'deployment.yaml', credentialsId: env.CREDENTIALS_ID, verifyDeployments: true])
            }
        }
    }    
}

