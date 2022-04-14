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
                pwd
                cp -prf /var/lib/jenkins/workspace/mytest/show /home/belhard/test
                cd /home/belhard/test/show
                ./wordpress.sh
                '''
            }
        }
    }
    post { 
        unsuccessful { 
                    sh '''cd /home/belhard/test/ && git config --global user.email "you@example.com" && git config --global user.name "ArtsiomBeladzedau" && git revert HEAD'''
                    sh '''
                      cp -prf /var/lib/jenkins/workspace/mytest/show /home/belhard/test
                      cd /home/belhard/test/show
                      ./wordpress.sh
                    '''    
                    sh """
                     curl -X POST https://api.telegram.org/bot5225721637:AAGxHahiWY-ZW022mgtoLsMNJS6CjoFCT6o/sendMessage -d "chat_id=1108837141" -d text="Job Jenkins unsuccessful & revert Git"
                    """
                    }
             // добавить удаление воркспейса!!
    }    
}
