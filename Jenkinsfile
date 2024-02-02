pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Java-Techie-jt/devops-automation']]])
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
                   docker.withRegistry('https://registry.hub.docker.com', 'docker-pwd') {
                        // Your Docker build and push steps here
                    
                        docker.withRegistry([credentialsId: 'docker-pwd', url: 'https://registry.hub.docker.com']) {
                            docker.image('tabasumkhan534/devops-integration').push()

}
                   sh 'docker push tabasumkhan534/devops-integration'
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
