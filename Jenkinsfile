pipeline{

agent any

tools {
  maven "maven3.8.4"
}

triggers {
  //Poll SCM
  pollSCM('* * * * *')
  //Build Periodically
  //cron('* * * * *')
}

options {
  timestamps()
  buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5')
}


stages{

//Get the code from GitHUb Repo
stage('CheckoutCode'){
steps{
git branch: 'development', credentialsId: '38f36fc3-1301-4d32-ab20-9ef99c485bee', url: 'https://github.com/Mithun-Software-Solutions/maven-web-application.git'
}
}

//Using Maven doing the Build
stage('MavenBuild'){
steps{
sh "mvn clean package"
}
}

//ExecuteSonarQubeReport
stage('ExecuteSonarQubeReport'){
steps{
sh "mvn sonar:sonar"
}
}

//UploadArtifactIntoNexusRepo
stage('UploadArtifactIntoNexusRepo'){
steps{
sh "mvn deploy"
} 
}

//DeployAppIntoTomactServer
stage('DeployAppIntoTomactServer'){
steps{
sshagent(['8609a0f5-8ab3-407a-b80d-557859091f66']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.184.111:/opt/apache-tomcat-9.0.58/webapps/"
}
}
}

}//stages closing

post {
 
  success {
emailext body: '''Build is over .....Success


Regards,
Mahesh ''', subject: 'Build is over', to: 'mahesh.indians15@gmail.com'
  }
  failure {
emailext body: '''Build is over .....Failure


Regards,
Mahesh ''', subject: 'Build is over', to: 'mahesh.indians15@gmail.com'
  }
}


}//pipeline closing
