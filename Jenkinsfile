node
{
    
def mavenHome = tool name: "maven-3.6.3"

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '3', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])

stage('checkoutcode')
{
git branch: 'development', credentialsId: '64e585c2-3d00-449e-9aeb-b21dd192172b', url: 'https://github.com/priy37/maven-web-application.git'
}

stage('Build')
{
sh "${mavenHome}/bin/mvn clean package"
}

stage('Execute sonarqube report')
{
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('upload Artifacts into Nexus')
{
sh "${mavenHome}/bin/mvn deploy"
}

stage('Deploy application into Tomcatserver')
{
sshagent(['17e58f74-0682-407d-82c8-033336934870']) {
  sh "scp -o  StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.1.106.67:/opt/apache-tomcat-9.0.43/webapps/"
}
}

 stage('Email notification')
{
emailext body: '''Build over...
regards,
priya''', replyTo: 'gandhalarsihitha57319@gmail.com', subject: 'Build over...', to: 'rishitharaji98@gmail.com'
}
   
}
