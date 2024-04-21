pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('Sonar Analysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=buggweppapp -Dsonar.organization=buggweppapp -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=224826eb1838f43a892d112bdcc81e54a0c3138f'
			}
    }

	stage('Run SCA Analysis Snyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }

	stage('Build Docker Image') { 
            steps { 
               withDockerRegistry([credentialsId: "DOCKER_LOGIN", url: ""]) {
                 script{
                 app =  docker.build("docker_repo")
                 }
               }
            }
    }

	stage('Push Docker Image to ECR ') {
            steps {
                script{
                    docker.withRegistry('https://342277402700.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:AWS_CREDENTIALS') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
