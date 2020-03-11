properties([
    buildDiscarder(logRotator(numToKeepStr: '3')),
    pipelineTriggers([
        pollSCM('* * * * *')
    ])
])

node('master'){  
    
    def mavenHome=tool name: "mavenv3.1.1.1", type: "maven"
    
 stage('Checkout the code') {
   git branch: 'master', credentialsId: '4d6512c4-c101-4f43-aac5-5860f5d9e20c', url: 'https://github.com/MithunTechnologiesDevOps/maven-web-application.git'  
 }
 
 stage('Build')
 {
  sh  "${mavenHome}/bin/mvn clean package"
 }
 
 stage('SonarQube Report')
 {
  sh  "${mavenHome}/bin/mvn sonar:sonar"
 }
   
  
  stage('Upload Artifacts into Nexus')
 {
  sh  "${mavenHome}/bin/mvn deploy"
 }
 
 stage('DeplotoTomcat'){
     
     sh "cp $WORKSPACE/target/*.war /opt/apache-tomcat-9.0.16/webapps/"
 }
 
 stage('EmailNotification'){
    mail bcc: '', body: '''Build Done

Regards,
Mithun Technologies
9980923226''', cc: 'devopstrainingblr@gmail.com', from: '', replyTo: '', subject: 'Build Notification', to: 'devopstrainingblr@gmail.com' 
     
 }
 
 stage("SlackNotification"){
     slackSend baseUrl: 'https://devops-team-bangalore.slack.com/services/hooks/jenkins-ci/', channel: 'build-notification', message: 'Build done through', tokenCredentialId: '12797dc5-eb70-4f19-8e05-8c07bc58d79d'
 }
}
