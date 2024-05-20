node{

def mavenHome = tool name: 'maven-3.9.6'

echo "Job name is: $JOB_NAME" 
echo "Node name is: $NODE_NAME"
echo "Jenkins Home url is: $JENKINS_HOME"

stage("CheckOutCode")
{
git branch: 'development', credentialsId: 'e1d4c644-e590-4c3c-ad70-3fc60a093466', url: 'https://github.com/sonalirallabandi/maven-web-application.git'
}

stage("Build")
{
sh "${mavenHome}/bin/mvn clean package"
}


stage("ExecuteSonarQubeReport")
{
sh "${mavenHome}/bin/mvn clean sonar:sonar"
}

stage("UploadArtifactsIntoNexus")
{
sh "${mavenHome}/bin/mvn clean deploy"
}
stage('DeployAppIntoTomcatServer'){
sshagent(['5aaa6a38-bc93-485e-836c-943d18ee39cf']) {
   sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@34.207.207.232:/opt/tomcat/webapps/"
}
}
}
