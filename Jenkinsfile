pipeline{
	agent any
	tools {
			maven 'Maven3'
	}

	stages{
		stage('Maven Build/package'){
			steps{
			sh 'mvn clean package'
			}
		}
	
	}
}
