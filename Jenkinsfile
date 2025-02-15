pipeline {
    agent any
    tools{
        maven 'maven'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ajayab05/devops-automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t ajayab05/kubernetes .'
                }
            }
        }
        stage('Push image to dockerhub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPass')]) {
                    sh 'docker login -u ajayab05 -p ${dockerPass}'
                        
                    }
                    sh 'docker push ajayab05/kubernetes'
                }
            }
        }
        stage('Deploy to K8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubeconfig')
                }
            }
        }
    
    }    
}
