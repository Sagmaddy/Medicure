pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Sagmaddy/Medicure/', branch: 'master'
            }
        }
        stage('Build Project') {
            steps {
                sh 'mvn clean package'
            }
        }
         stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t sagmaddy/medicure:v1 .'
                    sh 'docker images'
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhubpass', variable: 'dockerhubpass')]) {
                    sh 'docker login -u sagmaddy -p ${dockerhubpass}'
                }
                sh 'docker push sagmaddy/medicure:v1'
            }
        }

    }
}
