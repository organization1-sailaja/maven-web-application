node{
    def mavenHome = tool name: "Maven3.8.5"
    
    echo "Job Name is : ${JOB_NAME}"
    echo "Build Number is : ${BUILD_NUMBER}"
    
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5'))])
    properties([pipelineTriggers([pollSCM('* * * * *')])])
    
    stage('Checkoutcode'){
     git branch: 'development', credentialsId: 'b63870f2-dd31-4c46-ab58-ab5deb585d2a', url: 'https://github.com/organization1-sailaja/maven-web-application.git'
    }
    
    stage('Build'){
        sh "$mavenHome/bin/mvn package"
    }
    
    stage('SonarQubeReport'){
        sh "$mavenHome/bin/mvn sonar:sonar"
    }
    
    stage('ArtifactRepo'){
        sh "$mavenHome/bin/mvn deploy"
    }
    
    stage('DeploytoTomcat'){
    sshagent(['45a9a2dd-974f-4c29-81d1-693369fa7235']) {
        sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.2.9.198:/opt/apache-tomcat-10.0.23/webapps"
    }    
    }
    
}
