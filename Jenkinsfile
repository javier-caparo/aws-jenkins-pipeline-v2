pipeline {
	
	agent  { node { label "node" } }

	node {

		env.AWS_ECR_LOGIN=true
		def newApp
		def registry = 'javiercaparo/aws-jenkins-pipeline-v2'
		def registryCredential = 'dockerhub'
		
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
			docker.withRegistry( 'https://' + registry, registryCredential ) {
				def buildName = registry + ":$BUILD_NUMBER"
				newApp = docker.build buildName
				newApp.push()
			}
		}
		stage('Registring image') {
			docker.withRegistry( 'https://' + registry, registryCredential ) {
				newApp.push 'latest2'
			}
		}
		stage('Removing image') {
			sh "docker rmi $registry:$BUILD_NUMBER"
			sh "docker rmi $registry:latest"
		}
		
	}
}