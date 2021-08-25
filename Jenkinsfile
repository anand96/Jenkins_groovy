pipeline {
    agent any 
    environment {
        //once you sign up for Docker hub, use that user_id here
		registry = "a96nand/myassigmnet"
        //- update your credentials ID after creating credentials for connecting to Docker Hub
        registryCredential = 'dokcercredentialID'
        dockerImage = ''
    }
    
    stages {
		
		//Cloning the repo from the url
        stage('Cloning Git') {
            steps {
                git 'credentilalsID: 'git_credentials 'url:'https://github.com/anand96/spring-boot-web-application-sample.git'
            }
        }
		
		//Building  the project 
		stage('Build_Project') {
            steps {
                sh 'mvn clean install'
            }
        }
		
    
		// Building Docker images
		stage('Building image') {
		  steps{
			script {
				dockerImage = docker.build registry
			}
		  }
		}
		
		 // Uploading Docker images into Docker Hub
		stage('Upload Image') {
		 steps{    
			 script {
					docker.withRegistry( '', registryCredential ) {
					dockerImage.push()
				}
			}
		  }
		}
		
		 // Stopping Docker containers for cleaner Docker run
		 stage('docker stop container') {
			 steps {
				sh 'docker ps -f name=myassigmnet -q | xargs --no-run-if-empty docker container stop'
				sh 'docker container ls -a -fname=myassigmnet -q | xargs -r docker container rm'
			 }
		   }
		
		
		// Running Docker container, make sure port 8096 is opened in 
		stage('Docker Run') {
		 steps{
			 script {
				dockerImage.run("-p 8081:5000 --rm --name myassigmnet")
			 }
		  }
		}
	}
}