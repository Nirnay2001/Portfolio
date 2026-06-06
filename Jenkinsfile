pipeline {
    agent any
    tools{
        nodejs 'node18'
    }
    environment{
        EC2_IP= "100.24.10.145"
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
        stage("wait for deployment complete"){
            steps{
                sleep time: 30, unit: 'SECONDS'
            }
        }
        stage('Pre-condition run for tes'){
            steps{
                sh 'rm -rf Portfolio-test'
            }
        }
        stage('version check') {
            steps {
                sh 'node --version'
                sh 'npm --version'
                sh 'git --version'
            }
        }
        stage('test repo clone'){
            steps{
                sh 'git clone https://github.com/Nirnay2001/Portfolio-test.git'
            }
        }
        stage('Install Dependencies'){
            steps{
                dir('Portfolio-test'){
                    sh 'npm ci'
                }
            }
        }
        stage('Install browser'){
            steps{
                dir('Portfolio-test'){
                    sh 'npx playwright install'
                }
            }
        }
        stage('run test'){
            steps{
                dir('Portfolio-test'){
                    sh 'npx playwright test --reporter=html'
                }
            }
        }
    }
    post {
        always {
            dir('Portfolio-test'){
                publishHTML([
                    allowMissing: true,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'playwright-report',
                    reportFiles: 'index.html',
                    reportName: 'Playwright Report'
                ])
            }
        }
    }
}
