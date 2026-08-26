node
{
    //   /var/lib/jenkins/tools/hudson.tasks.Maven_MavenInstallation/Maven-3.9.16
   def mavenHome=tool name: "Maven-3.9.16"
   echo "git branch Name: ${env.BRANCH_NAME}"
   echo "build number: ${env.BUILD_NUMBER}"
  
   try
   {
   stage('git checkout')
   {
      notifyBuild('STARTED')
     git branch: 'Development', url: 'https://github.com/RajDevops-SRE-AIOPS/maven-webapplication-project-kkfunda.git'
   }
   stage('compile')
   {
    sh "${mavenHome}/bin/mvn compile"
   }

   stage('Build')
   {
    sh "${mavenHome}/bin/mvn clean package"
    
   }
   stage('SQ Report')
   {
    sh "${mavenHome}/bin/mvn sonar:sonar"
   }

   stage('Deploy Into Nexus')
   {
    sh "${mavenHome}/bin/mvn clean deploy"
   }

    stage('Deploy to Tomcat') 
    {
      
      sh """

      curl -u kk:password \
--upload-file /var/lib/jenkins/workspace/Scripted-Way-Pipeline/target/maven-web-application.war \
"http://13.232.165.81:8080/manager/text/deploy?path=/maven-web-application&update=true"
          
        """
    }
   
   }
   catch (e) {
   
       currentBuild.result = "FAILED"

  } finally {
    // Success or failure, always send notifications
    notifyBuild(currentBuild.result)       //function calling
  }
	
}  //node ending

def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#278EF5'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel: '#jenkins-dev')
  
}
