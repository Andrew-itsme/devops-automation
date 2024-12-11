pipeline {
    agent any
    tools{
        maven 'Maven3'
    }
    stages{
        stage('Build Maven'){
            steps{
                git branch: 'main', url: 'https://github.com/Andrew-itsme/devops-automation'
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t dattlt269/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u dattlt269 -p ${dockerhubpwd}'
}
                   sh 'docker push dattlt269/devops-integration'
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