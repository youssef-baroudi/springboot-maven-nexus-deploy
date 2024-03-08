pipeline 
{
      //Directives
      //agent any 
      agent
      {
          label 'node2'
      }
      environment 
      {
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
        GroupId = readMavenPom().getGroupId()
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
        
        // Stage 4 : Print some information
        stage ('Print Environment variables')
        {
                    steps {
                        echo "Artifact ID is '${ArtifactId}'"
                        echo "Version is '${Version}'"
                        echo "GroupID is '${GroupId}'"
                        echo "Name is '${Name}'"
                    }
                }

        // Stage3 : Publish the artifacts to Nexus
        /*stage ('Publish to Nexus')
        {
            steps 
            {
                script 
                {
                
                nexusArtifactUploader artifacts: 
                [[artifactId: "${ArtifactId}", 
                classifier: '', 
                file: 'target/springboot-maven-course-micro-svc-0.0.4-SNAPSHOT.jar', 
                type: 'jar']], 
                credentialsId: 'Nexus-credential', 
                groupId: "${GroupId}", 
                nexusUrl: '192.168.1.211:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: "${NexusRepo}", 
                version: "${Version}"
             }
            }
        }*/
      }
}