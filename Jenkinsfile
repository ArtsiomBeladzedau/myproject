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
                cp -pr /home/belhard/project/show /home/belhard/
                /home/belhard/show/wordpress.sh
                '''
            }
        }
    }
    post { 
        unsuccessful { 
                    sh '''cd /home/belhard/project/ && git config --global user.email "you@example.com" && git config --global user.name "ArtsiomBeladzedau" && git revert HEAD'''
                    sh '''
                      rm -rf /home/belhard/show
                      cp -pr /home/belhard/project/show /home/belhard/
                      /home/belhard/show/wordpress.sh
                    '''    
                    /sh """
                   /  curl -X POST https://api.telegram.org/bot5225721637:AAGxHahiWY-ZW022mgtoLsMNJS6CjoFCT6o/sendMessage -d "chat_id=1108837141" -d text="Job Jenkins unsuccessful & revert Git"
                   / """
                    }
              // cleanWs()
                }
           
    }    
