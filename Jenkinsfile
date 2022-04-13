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
        stage("Start CMS") {
            steps {
                sh '''
                rm -rf /home/belhard/demo
                cp -pr /home/belhard/workspace/myproject/Viktor_Bri/XX.Final_Project/demo /home/belhard/
                /home/belhard/demo/start_web.sh
                '''
            }
        }
    }
    post { 
        unsuccessful { 
                    sh '''cd /home/belhard/workspace/myproject/ && git config --global user.email "you@example.com" && git config --global user.name "ViktorB" && git revert HEAD'''
                    sh '''
                      rm -rf /home/belhard/demo
                      cp -pr /home/belhard/workspace/myproject/Viktor_Brile/XX.Final_Project/demo /home/belhard/
                      /home/belhard/demo/start_web.sh
                    '''    
                    sh """
                     curl -X POST https://api.telegram.org/bot5225721637:AAGxHahiWY-ZW022mgtoLsMNJS6CjoFCT6o/sendMessage -d "chat_id=1108837141" -d text="Job Jenkins unsuccessful & revert Git"
                    """
                    }
              // cleanWs()
                }
           
    }    
