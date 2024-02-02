pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Tabbukhan/Dev-Automation']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t tabasumkhan534/devops-integration .'
                }
            }
        }
        
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'docker-pwd', variable: 'docker-pwd')]) {
                   sh 'docker login -u tabasumkhan534 -p ${docker-pwd}'
                   }
                   }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
