pipeline{
	agent any
	tools {
			maven 'Maven3'
	}

	stages{
		stage('Git Checkout'){
			steps{
			git credentialsId: 'Github', url: 'https://github.com/madhavi3110/pets-app'
			}
		}
		stage('Maven Build/package'){
			steps{
			sh 'mvn clean package'
			}
		}
	
	}
}
