pipeline {
    agent any
     stages{
        stage('fetch code') {
          steps{
              git branch: 'vp-rem', url: "https://github.com/devopshydclub/vprofile-project.git"
          }  
        }

    stages{
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
            post {
                success {
                    echo "Now Archiving."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Test'){
            steps {
                sh 'mvn test'
            }

        }


        stage ('copy artifact to Nexus job'){
          steps{
            sh 'cp /home/jenkins/workspace/vprofile-pipeline/vprofile-v2.war /home/jenkins/workspace/nexus-versioning
            }
        }
         post {
            success {
               echo 'Artifact copied'
                }
             }

        stage ('copy artifact to deploy job'){
          steps{
            sh 'cp /home/jenkins/workspace/vprofile-pipline/vprofile-v2.war /var/lib/jenkins/workspace/DEPLOY
            }
        }
         post {
            success {
               echo 'Artifact copied'
                }
             }
        stage ('nexus versioning') {
          steps {
            build job: 'nexus-versioning'
             }
          }
        stage ('Deploy')  {
         steps {
             build job: 'DEPLOY'
            }
        } 

   
    }
}
