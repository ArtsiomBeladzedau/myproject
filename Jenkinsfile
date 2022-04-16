pipeline {
  agent any
  stages {
        stage("Initial config") {
            steps {
                script {
                    properties([pipelineTriggers([pollSCM('* * * * *')])])
                }
            }
        }
        stage("Checkout Git") {
            steps {
                git branch: 'master', url: 'https://github.com/ArtsiomBeladzedau/myproject.git'
            }
        }
        stage("Start Wordpress") {
            steps {
                sh '''
                rm -rf /home/belhard/show
                cp -prf /var/lib/jenkins/workspace/mytest/show /home/belhard/test
                cd /home/belhard/test/show
                ./wordpress.sh
                '''
                sh """
                 curl -X POST https://api.telegram.org/bot{$TOKEN}/sendMessage -d chat_id=${CHAT_ID} -d text="Job Jenkins successful"
                """

            }
        }
    }
    post { 
        unsuccessful { 
                    sh '''cd /home/belhard/myproject/show && git config --global user.email "you@example.com" && git config --global user.name "ArtsiomBeladzedau" && git revert HEAD'''
                    sh '''
                      rm -rf /home/belhard/show
                      cp -prf /var/lib/jenkins/workspace/mytest/show /home/belhard/test
                      cd /home/belhard/test/show
                      ./wordpress.sh
                    '''    
                //    sh """
                //     curl -X POST https://api.telegram.org/bot5225721637:AAGxHahiWY-ZW022mgtoLsMNJS6CjoFCT6o/sendMessage -d "chat_id=1108837141" -d text="Job Jenkins unsuccessful & revert Git"
                //    """
         }// добавить удаление ворспейса!!
         
         always {
             cleanWs()
        }
    }    
}
