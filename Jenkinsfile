pipeline
{
	agent any
	tools
	{
	maven "maven"
	}
	stages{
		stage('Code Checkout'){
			steps{
				git branch: 'main', url: 'https://github.com/Suma-SBI/Docker-Web-App.git'

			}
		}
		stage('Execute Maven'){
			steps{
				sh 'mvn package'
			}
		}

		stage('War Deploy into Nexus'){
			steps{
				sh 'mvn deploy'
			}
		}


		stage("Copying the War file to Job Location"){
			steps{
				sh 'cp /var/lib/jenkins/workspace/4444/target/*.war /var/lib/jenkins/workspace/4444' 

		}
	}
	    stage("Docker Image Build"){
			steps{
			sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID . '
			}
	             }
	   stage("Docker Image taging"){
			steps{
			sh 'docker image tag $JOB_NAME:v1.$BUILD_ID thanish/$JOB_NAME:v1.$BUILD_ID'
			
		}

		}
		stage("push Image: DOCKERHUB"){
             steps{

                withCredentials([string(credentialsId: 'DockerPassword', variable: 'DockerPassword')]) {
                sh 'docker login -u lathasuma -p ${DockerPassword}'
                sh 'docker image push lathasuma/$JOB_NAME:v1.$BUILD_ID'
                
              }
         }
      }
		stage("Email Nontification"){
              steps{
			mail bcc: '', body: 'Hi Welcome to Jenkins Email Alters', cc: '', from: '', replyTo: '', subject: 'Jenkins Job', to: 'ravindra.devops@gmail.com'
		}
          }
	}
}



