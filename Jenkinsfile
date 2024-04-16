pipeline {
    agent any

    environment {
        IMAGE_NAME = 'pranjalsrivastava/newjendocimg'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout')
        {
            steps{
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build WebApplication1/WebApplication1.csproj --configuration Release'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Docker Build')
        {
            steps {
                script{
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Dcoker Push') {
            steps {
                script{
                    docker.withRegistry('https://index.docker.io/v1/', 'docker_login'){
                        docker.image("${IMAGE_NAME}:${IMAGE_TAG}").push()
                    }
                }
                
            }
        }

        stage('Run Container') {
            steps {
                script{
                    sh 'docker rm -f ${IMAGE_NAME} || true'
                    sh 'docker run -d --name storeapp -p 8000:80 ${IMAGE_NAME}:${IMAGE_TAG}'
                    }
                }
                
            }
        }
    }