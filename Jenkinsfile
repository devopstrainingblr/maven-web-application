node{
    def mavenHome= tool name: "Maven 3.6.3"
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')),
    pipelineTriggers([pollSCM(ignorePostCommitHooks: true, scmpoll_spec: '* * * * *')])])
stage('Checkout File from SCM')
{
    checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [],
userRemoteConfigs: [[credentialsId: 'c897140a-285c-4017-a50e-8265887e47a8', url: 'https://github.com/World-123/maven-web-application.git']]])
}
stage ('Build the code')
{
    sh "${mavenHome}/bin/mvn clean package"
}
stage ('Create sonar report')
{
    sh "${mavenHome}/bin/mvn sonar:sonar"
}
stage ('Deploy to nexus')
{
    sh "${mavenHome}/bin/mvn deploy"
}
stage('Deploy to Tomcat server')
{
    sshagent(['1bad6e6f-9f42-43a0-ae0b-8e807e89329b']) 
    {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.127.100.233:/opt/tomcat9/webapps"
    }
}

}
