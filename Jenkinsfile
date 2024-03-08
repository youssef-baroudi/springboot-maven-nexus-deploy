pipeline 
{
      //agent any 
      agent
      {
          label 'node2'
      }
      environment 
      {
          registry = 'https://hub.docker.com/repositories/1youssefbaroudi1'
          registryCredential = 'jenkins-dockerhub-login-credentials'
          dockerimage = ''
      }
      stages
      {
          stage("Checkout the project") 
          {
            steps
            {
                git branch: 'main', url: 'https://github.com/youssef-baroudi/springboot-maven-nexus-deploy.git'
            } 
          }
          stage("Build the package")
          {
              steps 
              {
                  sh 'mvn clean package'
              }
          }
          stage ('Test')
          {
            steps 
            {
                echo ' testing......'
            }
           }
      }
}