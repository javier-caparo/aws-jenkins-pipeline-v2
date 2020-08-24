pipeline {
	
	agent { label 'master' }

  	tools { nodejs "nodejs" }

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
		stage('Building dependencies') {
			steps{
				sh 'npm install'
			}
			
		}
		stage('Run Tests') {
			parallel {
				stage('Mocha Unitary Tests') {
					steps {
						sh 'npm test'
					}
				}
				stage('Lint Tests eslint') {
					steps {
						sh 'npm run lint'
					}
				}
			}
		}
		stage('Building docker image') {
			steps{
        		script {
          			dockerImage = docker.build registry + ":$BUILD_NUMBER"
        		}
      		}
		}
		stage('Pushing image to dockerhub') {
			steps{
        		script {
          			docker.withRegistry( '', registryCredential ) {
            			dockerImage.push()
          			}
        		}
      		}
		}
		stage('Removing image locally') {
			steps{
        		sh "docker rmi $registry:$BUILD_NUMBER"
      		}
		}
		
	}
}