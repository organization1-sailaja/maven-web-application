node{
def mavenHome = tool name: "maven3.8.5"

stage('CheckoutCode'){
git branch: 'development', credentialsId: 'a3b8426d-2ff4-48a8-95ba-79d97d091786', url: 'https://github.com/organization1-sailaja/maven-web-application.git'
}

stage('Build'){
sh "$mavenHome/bin/mvn clean package"
}

stage('SonarQubeReport'){
sh "$mavenHome/bin/mvn sonar:sonar"
}

stage('UploadArtifactToNexus'){
sh "$mavenHome/bin/mvn deploy"
}

stage('DeployAppToTomcat'){
sshagent(['0e40c6d9-fb97-4749-aec3-abdd7e54c4fc']) {
  sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.233.33.236:/opt/apache-tomcat-9.0.59/webapps"
}
}

}
