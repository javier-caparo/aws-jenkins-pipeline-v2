pipeline {
	
	agent  { node { label "node" } }

	environment { 
        env.AWS_ECR_LOGIN=true
		registry = 'javiercaparo/aws-jenkins-pipeline-v2'
		registryCredential = 'dockerhub'
    }
	stages {
		
		def newApp
		
		stage('Git') {
			git 'https://github.com/jfcb853/aws-jenkins-pipeline-v2'
		}
		stage('Build') {
			sh 'npm install'
		}
		stage('Test') {
			sh 'npm test'
		}
		stage('Building image') {
			docker.withRegistry( 'https://' + $registry, $registryCredential ) {
				def buildName = $registry + ":$BUILD_NUMBER"
				newApp = docker.build buildName
				newApp.push()
			}
		}
		stage('Registring image') {
			docker.withRegistry( 'https://' + $registry, $registryCredential ) {
				newApp.push 'latest2'
			}
		}
		stage('Removing image') {
			sh "docker rmi $registry:$BUILD_NUMBER"
			sh "docker rmi $registry:latest"
		}
		
	}
}