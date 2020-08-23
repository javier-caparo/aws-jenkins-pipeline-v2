pipeline {
	
	agent any

	environment { 
        registry = 'javiercaparo/aws-jenkins-pipeline-v2'
		registryCredential = 'dockerhub'
		dockerImage = ''
    }
	stages {
		
		stage('Cloning Git') {
			steps {
				git 'https://github.com/jfcb853/aws-jenkins-pipeline-v2'
			}
		}
		stage('Buildind dependencies') {
			steps{
				sh 'npm install'
			}
			
		}
		stage('Testing - Unitary Tests') {
			steps {
				sh 'npm test'
			}
			
		}
		stage('Building image') {
			steps{
        		script {
          			docker.build registry + ":$BUILD_NUMBER"
        		}
      		}
		}
		stage('Registring image') {
			steps{
        		script {
          			docker.withRegistry( '', registryCredential ) {
            			dockerImage.push()
          			}
        		}
      		}
		}
		stage('Removing image') {
			steps{
        		sh "docker rmi $registry:$BUILD_NUMBER"
      		}
		}
		
	}
}