pipeline
{
  agent nodes  
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
          build job: 'Bsnl_qa_scripted pipeline'
        }
    }    
  }
}    
    
   
