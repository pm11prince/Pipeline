#!/usr/bin/env groovy
def app

pipeline {
  agent any
  //All parameters which will be used to run the pipeline.
  parameters {
		booleanParam(name: 'slackNotification', defaultValue: true, description: '') 
        booleanParam(name: 'emailNotification', defaultValue: true, description: '')
		
		}
  stages {
	
	
  stage('Build image') {
  
  steps {
  echo "Build image"
  script{
  if (params.slackNotification == true){
  slackNotification()
  }
  }
  
  }
    }

		
 stage('Create Bridge') {
 
 steps {
 echo "Create Bridge"
 }
		
		}	

				
 stage('Checkout code'){
 
 steps {
 echo "Checkout code"
 }
			}
			 
    }
    }
	
// function to handle the failed build notification.
		def notifyFailedBuild(String stage) {
		
		emailext(
		  to: emailextrecipients([[$class: 'DevelopersRecipientProvider'],[$class: 'CulpritsRecipientProvider'],[$class: 'RequesterRecipientProvider']]),,
		  subject: "Build Failed: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
		  body: "This email is to notify that Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' has been failed. Failed stage: [${stage}]"
		)
		}
		
// function to handle successful build notification.
		def notifySuccessBuild() {
		
		emailext(
		  to: emailextrecipients([[$class: 'DevelopersRecipientProvider'],[$class: 'CulpritsRecipientProvider'],[$class: 'RequesterRecipientProvider']]),,
		  subject: "Build Success: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
		  body: "This email is to notify that Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' has been completed successfully"
		)
		}
		
		//def slackNotification(String channel, String message){
		def slackNotification(){
		slackSend channel: '#cisco-pipeline',
                  color: 'good',
                  message: "The pipeline ${currentBuild.fullDisplayName} completed successfully."
		}

//function to clean docker container, volumes and network		
		def cleanup() {
		sh """
		docker ps -q -f status=exited | xargs --no-run-if-empty docker rm
		docker images -q -f dangling=true | xargs --no-run-if-empty docker rmi
		docker volume ls -qf dangling=true | xargs -r docker volume rm
		docker network rm spadelite${env.BUILD_NUMBER}
		"""
		}