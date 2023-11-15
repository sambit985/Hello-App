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
     options {
         timeout(time: 10, unit: 'MINUTES')
      }
     steps {
         script {
             echo 'Installing dependencies...'
             sh 'npm install'

             echo 'Building the app...'
             sh 'npm run build'
         }
     }
 }


         stage('Update Browserslist') {
            steps {
                script {
                    echo 'Updating Browserslist database...'
                    sh 'npx browserslist@latest --update-db'
                }
            }
        }

        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    script {
                        echo 'Installing SonarScanner...'
                        sh "npm install sonar-scanner"

                        echo 'Running SonarScanner...'
                        sh "npm run sonar-scanner"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying to the server...'
                    sh "rsync -azh --info=progress2 --info=name0 -e $SSH_COMMAND /var/lib/jenkins/workspace/ ubuntu@3.81.114.150:/home/ubuntu/host/"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
