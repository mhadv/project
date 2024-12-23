pipeline {
    agent any
    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'dockerhub', url: 'https://github.com/mhadv/WorkFlow.git'
            }
        }
        
        stage('Compile') {
            steps {
                sh "mvn compile"
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }
        
        stage('Docker Build') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub') {
                        sh 'docker build -t springboot-app .'
                    }
                }
            }
        }
        
        stage('Docker Tag and Push Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub') {
                        sh 'docker tag springboot-app omrajput/springboot-app:latest'
                        sh 'docker push omrajput/springboot-app:latest'
                    }
                }
            }
        }
        
        stage('Deploy to Container') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'dockerhub') {
                        sh "docker run -d -p 8090:8090 omrajput/springboot-app:latest"
                    }
                }
            }
        }
    }
}
