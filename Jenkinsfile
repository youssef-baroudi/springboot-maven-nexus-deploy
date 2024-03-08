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

        // Stage3 : Publish the artifacts to Nexus
        stage ('Publish to Nexus')
        {
            steps 
            {
                script 
                {

                def NexusRepo = Version.endsWith("SNAPSHOT") ? "VinaysDevOpsLab-SNAPSHOT" : "VinaysDevOpsLab-RELEASE"
                
                nexusArtifactUploader artifacts: 
                [[artifactId: 'springboot-maven-course-micro-svc', 
                classifier: '',
                file: 'target/springboot-maven-course-micro-svc-0.0.4-SNAPSHOT.jar', 
                type: 'jar']], 
                credentialsId: 'Nexus-credential', 
                groupId: 'com.cloudtechmasters', 
                nexusUrl: '192.168.1.211:8081/', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: 'springboot-maven-nexus-SNAPSHOT', 
                version: '0.0.4-SNAPSHOT'
             }
            }
        }
      }
}