pipeline('nodes'){

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
sshagent(['-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAhZVIGbP/ZxvAwn2+Up7mNDM0Msohq58Han2x4dt/XqN2GQ2g
VpQDnB63XEPzqcYOuetC2hc+8hwlfBqcL1a+D4ocMzUcodyGi1O6h/SjbNPNwcxg
Q221YrhEzAmV62l7ka27VDfK1MXNVG/an3uvN8igv5pcFXv6cNQBzVbpYFOJr5R/
5fa01YQ9SKaMYKSoYmWPKjNY9O/KPBmnMWbZ6nOdGn4c2M2MiI7/iE4qPH0L56iK
gxclBLpcl9WEqqtMYtrbdwIFnAEtLCkbae83/b5GDL31YKaIFB9tFifYkqK5qwWC
EiboaDBePC6tFMb3mgNaNrTkO57WkgmcgpZYdwIDAQABAoIBAFKEmPlnu4nSFw0N
6BfZmJL+XmxReOMvZcFezBedI90uVLG9PSK+ZEx4nQQd5dMmScqHjdBzM6sTwAQd
3AVSLC4lPq5TTNCgDWzb2ApMEPgVDrF8sqp0huvosHbqJPY7Pt4K2AF6RY2ScviJ
8B88pExtXv99Nwz+fQJdtI9H9PpPNwE8h5/11jkiGNWS3aIVrWhafCcVzR+dgL9m
bxJUdMENx18WpskvPYgMV0/8+aTis8LX3XDlWCMTR1y/jyxIY75rnIsoO8PLs6+B
OO+5L7z6MVIe0im1XUkkFyk/v8cZOZAGKFJ+G+rMWisc6X6ZJQ5oSN77hODxSNan
9+YHyqECgYEA45eU2gUwIWdJI4qJg2kBzoNTmO5dkOB0aDoCm4m87rdnMppGK1IF
RA8dlP15nM6FRaa8U4g/TZJCB7D2uTXFl0TaTh9zAIV5jZjYIcQ3/Ef5AMYqtTJT
CLPmVJIu+MoGRbyrb9z8rWll3AzFsYRPAI7yUANhXNZ9qHA1MoW/CeUCgYEAlkHD
Cck3wKMrR4bZGbswHvYqCdoWQSc1SoEfEshym0/EMvmkV2laSbFoHK96uS0jJkJ3
z2vwpf6tldTKvLYVIiq6pW39Tuu8XU9yaTaCmZZk1eeLgkvFdADHwG78LmETojyD
C40fNlxaBS+XzESLeAUWtdMUkrlLTwPACUa6AysCgYBzRS3N8ry63l4r0xns5b1V
hCxOE8RuAVDUDTWO44c+fMOW3I5XmJY0L1ezQ2JZ6juT2Gwf/qzZNA+fZ6C+k559
DBFpagJMLE4xSk2FZKVacHWMT9IHrfJiQQOSp+uEdIYSwgkuggW0KuK9PfbO/w0o
Yj4WCnBAnh5MtnArI5RrhQKBgFN+TWOltV5NDSKc0wySUKYTwb5hulYP9HPnFh44
1j5pb6unvuN3vl0OwLyX4gj+BPcgnjTbVQjYYRrN+K4uO8YVmkuMt+Jf6fary/ac
/Ktdv8CA/quzcRAJ0vWidm1LMj7Hg0Yq7/okDT2ueZpfSGSz5y+4EEmLv0Yz3kUJ
URmrAoGBAI6riTZxSmFYOQnFOCKkH5ZVBVdo2XADUb9j/fdyQHAb5jhcWERldm11
ciHSjEwZcV0JO14lTycB9VqNyAgkDwPGptscczTCOp7gGovgBIyCyhwU2sfYNBlA
ChM7NaIxAEq+FEyNeJj7HG9kU43l1tNp1IeP81jWQQhH76l0E8Ee
-----END RSA PRIVATE KEY-----
']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@15.206.159.22:/opt/apache-tomcat-9.0.58/webapps/"
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
