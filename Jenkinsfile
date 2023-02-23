pipeline{
    agent any
    stages {
        stage('git_checkout') {
            steps{
                
               git credentialsId: 'shankar', url: 'git@github.com:Shankargoud1/spring-boot-war-example.git'
            }
           }
         
            stage('Maven Build'){
            steps{
                sh "mvn clean package"
            }
        }       

        stage('Build Docker Image') {
            steps {
                script {
                  sh 'docker build -t dockerawsdevops1/docker-private-repo:latest .'
                }
            }
        }
        stage('Deploy Docker Image') {
            steps {
                script {
                 withCredentials([string(credentialsId: ''DOCKER_HUB', variable: ''DOCKER_HUB')]) {
                    sh 'docker login -u dockerawsdevops1 -p ${'DOCKER_HUB}'
                 }  
                 sh 'docker push dockerawsdevops1/docker-private-repo:latest'
                }
            }
        }
    
    stage('Deploy App on k8s') {
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
      
    
    
    
