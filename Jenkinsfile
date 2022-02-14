node{


properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], 
pipelineTriggers([pollSCM('* * * * *')])])

def mavenHome = tool name: "maven3.8.4"

//Get the code from GitHUb Repo
stage('CheckoutCode'){
git branch: 'development', credentialsId: '38f36fc3-1301-4d32-ab20-9ef99c485bee', url: 'https://github.com/Mithun-Software-Solutions/maven-web-application.git'
}

//Using Maven doing the Build
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

stage('UploadArtifactIntoNexusRepo'){
sh "${mavenHome}/bin/mvn deploy"
}

stage('DeployAppIntoTomactServer'){
sshagent(['8609a0f5-8ab3-407a-b80d-557859091f66']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.184.111:/opt/apache-tomcat-9.0.58/webapps/"
}
}


}//Node Close
