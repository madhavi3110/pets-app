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
	stages{
		stage('Nexus Deploy'){
			steps{
			nexusArtifactUploader 
				artifacts: [[artifactId: 'pets-app', classifier: '', file: 'target/pets-app.war', type: 'war']], 
				credentialsId: 'Nexus3', 
				groupId: 'in.Madhavi', 
				nexusUrl: '172.31.32.127:8081', 
				nexusVersion: 'nexus3', 
				protocol: 'http', 
				repository: 'pets-app-snapshot', 
				version: '2.2-SNAPSHOT'
			}
		}
	}
}
