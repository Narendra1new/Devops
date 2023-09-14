pipeline {
    agent {
        label 'slave_group'
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
                sh 'scp -r /home/ec2-user/slave/workspace/Devops/Devops/index.html ec2-user@172.31.32.198:/home/ec2-user'
              }
            }
        }
        stage('Build image') {
            steps {
                sshagent(credentials: ['Dockerkey']) {
                sh 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.32.198 "/home/ec2-user/build.sh "'
              }
            }
        }
        
    }
}
