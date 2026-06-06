pipeline {
    agent any
    
    environment{
        EC2_IP= "3.94.108.208"
        git_repo= "https://github.com/Nirnay2001/Portfolio.git"
    } 

    stages {
        stage('Pre-condition run'){
            steps{
                sh 'rm -rf Portfolio'
            }
        }
        stage('git version check') {
            steps {
                sh 'git --version'
            }
        }
        stage('git repo clone'){
            steps{
                sh 'git clone $git_repo'
            }
        }
        stage('deploy'){
            steps{
                dir('Portfolio'){
                    sshagent(['ec2-ssh']){
                        sh 'scp -o StrictHostKeyChecking=no index.html ubuntu@$EC2_IP:/tmp/'
                        sh 'ssh -o StrictHostKeyChecking=no ubuntu@$EC2_IP "sudo cp /tmp/index.html /var/www/html/index.html"'
                    }
                }
            }
        }
    }
}
