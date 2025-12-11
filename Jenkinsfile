pipeline
{
    agent any
    tools
    {
    maven "mvn"    
    }
  stages
   {
      stage('git checkout')
      {
        steps
        {
            notifyBuild('STARTED')
          git branch: 'feature2', url: 'https://github.com/Devops-aws848/maven-webapplication-project-kkfunda.git'  
        }
      }
     
     stage('compile')
     {
       steps
         {
          sh "mvn compile"
        }
    }
     stage('Build')
      {
        steps
        {
          sh "mvn clean package"  
        }
    }
    stage('Qualitycheck')
    {
        steps
        {
          sh "mvn sonar:sonar"  
        }
    }
    stage('Deploy to Nexus')
    {
        steps
        {
          sh "mvn clean deploy"  
        }
    }    
    stage('Deploy to App')
    {
        steps
        {
          sh """
          curl -u admin:password \
          --upload-file /var/lib/jenkins/workspace/theprint/target/maven-web-application.war \
          "http://52.66.247.188:8080/manager/text/deploy?path=/maven-web-application&update=trye" 
          """
        }
    }    
 }
        
}
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
  slackSend (color: colorCode, message: summary, channel: '#theprint,#offset')
  
}
