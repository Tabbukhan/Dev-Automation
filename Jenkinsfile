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
                    sh 'docker build -t tabasumkhan534/devops-integration:myimage .'
                }
            }
        }
        
        stage('Push image to Hub'){
            steps{
                withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerpwd')]) {
                    //echo "Executing docker login command..."
                    //sh 'docker login -u tabasumkhan534 -p $dockerpwd docker.io'
                    echo "Executing docker push command..."
                   }
                docker tag first-image tabasumkhan534/devops-integration:first-image
                sh 'docker push tabasumkhan534/devops-integration:first-image'
            }
        } 


        //Note able to create credentials for this in jenkins
        //stage('Deploy') {
            //steps {
              //  script{
                 //       docker.withRegistry('https://720766170633.dkr.ecr.us-east-2.amazonaws.com', 'ecr:us-east-2:aws-credentials') {
                 //   app.push("${env.BUILD_NUMBER}")
                  //  app.push("latest")
                   // }
                //}
            //}
        //}
    }
}

