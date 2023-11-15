pipeline {
    agent any

    environment {
        PATH = "$PATH:/usr/bin/node"
        SSH_COMMAND = "ssh -i /var/lib/jenkins/.ssh/id_rsa"
    }

    stages {
        stage('GetCode') {
            steps {
                git branch: 'master', url: 'https://github.com/sambit985/Hello-App.git'
            }
        }

       stage('Build') {
          steps {
             timeout(time: 10, unit: 'MINUTES') {
                 sh 'npm install'
                 sh 'npm run build'
            }
          }
        }


        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh "npm install sonar-scanner"
                    sh "npm run sonar-scanner"
                }
            }
        }

    stage('deploy') {
     steps {
            sh "rsync -azh --info=progress2 --info=name0 -e $SSH_COMMAND  /var/lib/jenkins/workspace/ ubuntu@3.81.114.150:/home/ubuntu/host/"
       }
     }
  }
}
