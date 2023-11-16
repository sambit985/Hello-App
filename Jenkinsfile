pipeline {
    agent any

    environment {
        PATH = "/var/lib/jenkins/.nvm/versions/node/v18.18.2/bin:$PATH"
        NVM_DIR = '/var/lib/jenkins/.nvm'
        SSH_COMMAND = "ssh -i /var/lib/jenkins/.ssh/id_rsa"
    }

    stages {
        stage('GetCode') {
            steps {
                git branch: 'master', url: 'https://github.com/sambit985/api-test.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    echo 'Installing Node.js and npm...'
                    sh 'env'
                    sh 'ls -la /var/lib/jenkins/.nvm'
                    sh 'export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" && nvm install 18'
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building the app...'
                    sh 'npm run build'
                }
            }
        } 

     stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh "npm install -g browserslist" // Install browserslist globally
                    sh "npx browserslist@latest --update-db" // Update Browserslist data
                    sh "npm install sonar-scanner"
                    sh "npm run sonar-scanner"
                }
            }
        } 
    
    stage('deploy') {
     steps {
           echo 'Delivering the app to AWS Server'
            sh "rsync -azh --info=progress2 --info=name0 -e $SSH_COMMAND  /var/lib/jenkins/workspace/ ubuntu@3.81.114.150:/home/ubuntu/host/"
       }
     }

    }
}
