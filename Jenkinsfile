pipeline {
    agent any
    tools{
        maven 'maven'
    }
   environment {
        DOCKER_HUB = credentials('DOCKER-CREDS')
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
                    sh 'docker build -t tabasumkhan534/devops-integration:myimage .'
                    //sh 'docker tag myimage tabasumkhan534/devops-integration:myimage'
                }
            }
        }

        stage('docker login'){
            steps{
            sh 'echo $DOCKER_HUB_PSW | docker login -u $DOCKER_HUB_USR --password-stdin'
            }
        }
        stage('Push image to Hub'){
            
            steps{
               // withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerpwd')]) {
                    //echo "Executing docker login command..."
                    //sh 'docker login -u tabasumkhan534 -p $dockerpwd docker.io'
                   //}
                echo "Executing docker push command..."
                sh 'docker push tabasumkhan534/devops-integration:myimage'
            }
        } 
        stage('trivy scan') {
           steps{
            sh "trivy tabasumkhan534/devops-integration:myimage"
          }
        }
    }
}

