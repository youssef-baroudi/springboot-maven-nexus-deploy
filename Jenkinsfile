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
        
        // Print some information
        stage ('Print Environment variables')
        {
            steps 
            {
                echo "Artifact ID is '${ArtifactId}'"
                echo "Version is '${Version}'"
                echo "GroupID is '${GroupId}'"
                echo "Name is '${Name}'"
            }
        }

        // Publish the artifacts to Nexus
        stage ('Publish to Nexus')
        {
            steps 
            {
                script 
                {
                    def NexusRepo = Version.endsWith("SNAPSHOT") ? "springboot-maven-nexus-SNAPSHOT" : "springboot-maven-nexus-RELEASE"

                    nexusArtifactUploader artifacts: 
                    [[artifactId: "${ArtifactId}", 
                    classifier: '', 
                    file: "target/${ArtifactId}-${Version}.jar", 
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
        }

        // Deploying the build artifact to Apache Tomcat
        stage ('Deploy to Tomcat')
        {
            steps 
            {
                echo "Deploying ...."
                sshPublisher(publishers: 
                [sshPublisherDesc
                (
                    configName: 'Ansible_Controller', 
                    transfers: 
                    [
                        sshTransfer
                        (
                                cleanRemote:false,
                                execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_as_tomcat_user.yaml -i /opt/playbooks/hosts --extra-vars "ansible_sudo_pass=admin123"',
                                execTimeout: 120000
                        )
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false
                )
                    ])
            
            }
        }
      }
}