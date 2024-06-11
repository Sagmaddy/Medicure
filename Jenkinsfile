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
        stage('Deploy') {
            steps {
                script {
                    // Ensure Jenkins has sudo privileges to run docker
                    sh 'sudo docker run -itd -p 8090:8082 sagmaddy/medicure:v1'
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
