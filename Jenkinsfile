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
