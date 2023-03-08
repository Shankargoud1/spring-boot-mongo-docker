 pipeline{
    agent any
    stages {
        stage('git_checkout') {
            steps{
                
               git credentialsId: 'shankar', url: 'git@github.com:Shankargoud1/spring-boot-mongo-docker.git'
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
                 withCredentials([string(credentialsId: 'DOCKER_HUB', variable: 'DOCKER_HUB')]) {
                    sh 'docker login -u dockerawsdevops1 -p ${DOCKER_HUB}'
                 }  
                 sh 'docker push dockerawsdevops1/docker-private-repo:latest'
                }
            }
        }
      stage('Deploy App on kubernetes') {
         steps {
            sshagent(['kubernetes']) {
            sh "scp -o StrictHostKeyChecking=no pod.yml ec2-user@172.31.8.15:/home/ec2-user"
            script {
                try{
                    sh "ssh ec2-user@172.31.8.15 kubectl create -f ."
                }catch(error){
                    sh "ssh ec2-user@172.31.8.15 kubectl create -f ."
            }
            }
        }
         }  
        
    }
      }
    }