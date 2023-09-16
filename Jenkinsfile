pipeline {
    agent {
        label 'slave_group'
    }
    environment {
        DOCKERHUB_CRED = credentials('dockerhub')
    }
    stages {
        stage('Clean') {
            steps {
                sh "rm -rf Devops"
            }
        }
        stage('Git clone') {
            steps {
                sh "git clone https://github.com/Narendra1new/Devops.git"
            }
        }
        stage('Deploy code') {
            steps {
                sshagent(credentials: ['Dockerkey']) {
                sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.32.198 '
                sh 'scp -r index.html ec2-user@172.31.32.198:/home/ec2-user'
              }
            }
        }
        stage('Build image') {
            steps {
                sshagent(credentials: ['Dockerkey']) {
                sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.32.198 "/home/ec2-user/buildcompose.sh "'
              }
            }
        }
        stage('dockerlogin') {
            steps {
                 sshagent(credentials: ['Dockerkey']) {
                 sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.32.198 "echo $DOCKERHUB_CRED_PSW  | sudo  docker login -u $DOCKERHUB_CRED_USR --password-stdin && sudo docker push 9989265439/my-nginx:latest"'
                 }
            }
        }
        
    }
}
