pipeline {
    agent any
    
    environment{
        EC2_IP= "54.81.186.221"
        git_repo= "https://github.com/Nirnay2001/Portfolio.git"
    } 

    stages {
        stage('deploy'){
            steps{
                sshagent(['ec2-ssh']){
                        sh 'ssh -o StrictHostKeyChecking=no ubuntu@$EC2_IP "rm -rf Portfolio && git clone $git_repo"'
                        sh 'ssh -o StrictHostKeyChecking=no ubuntu@$EC2_IP "sudo docker stop portfolio || true && sudo docker rm portfolio || true"'
                        sh 'ssh -o StrictHostKeyChecking=no ubuntu@$EC2_IP "cd Portfolio && sudo docker build -t portfolio:v${BUILD_NUMBER} ."'
                        sh 'ssh -o StrictHostKeyChecking=no ubuntu@$EC2_IP "sudo docker run -d --name portfolio -p 80:80 portfolio:v${BUILD_NUMBER}"'
                    }
            }
        }
    }
}
