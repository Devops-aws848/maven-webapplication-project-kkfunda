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
            git branch: 'feature1', url: 'https://github.com/Devops-aws848/maven-webapplication-project-kkfunda.git'
        }
      }
     
     stage('compile')
     {
       steps
         {
          sh "mvn clean compile"
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
          --upload-file /var/lib/jenkins/workspace/Bsnl/target/maven-web-application.war \
          "http://13.234.67.81:8080/manager/text/deploy?path=/maven-web-application&update=trye" 
          """
        }
    } 
       stage('Build_bsnl_qa')
    {
        steps
        {
          build job: 'qa_feature1'
        }
    }    
 }
    
    post {
  success {

    script
    {
     notifyBuild(currentBuild.result)
    }
    
  }
  failure {

  script
  {
    notifyBuild(currentBuild.result)

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
  slackSend (color: colorCode, message: summary, channel: '#bsnl,#theprint')
  
}
