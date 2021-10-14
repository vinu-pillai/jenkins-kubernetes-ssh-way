pipeline{
    agent any
    stages {
        stage('SCM Checkout') {
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'devopshint', url: 'https://github.com/devopshint/jenkins...]]])

             
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                  sh 'docker build -t devopshint/nodejsapp-1.0:latest .'
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u devopshint -p ${dockerhubpwd}'
                 }  
                 sh 'docker push devopshint/nodejsapp-1.0:latest'
                }
            }
        }
    
    stage('Deploy on Kubernetes') {
      steps {
            sshagent(['k8s']) {
            sh "scp -o StrictHostKeyChecking=no nodejsapp.yaml ubuntu@IPofk8scluster:/home/ubuntu"
            script {
                try{
                    sh "ssh ubuntu@IPofk8scluster kubectl create -f ."
                }catch(error){
                    sh "ssh ubuntu@IPofk8scluster kubectl create -f ."
            }
}
        }
      
    }
    }
    }
}