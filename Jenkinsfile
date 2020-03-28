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

		stage('Nexus Deploy'){
			steps{
				script{
			  		def pomFile = readMavenPom file: 'pom.xml'
					def version = pomFile.version  
					def nexusRepo = version.endsWith("SNAPSHOT") ? "pets-app-snapshot" : "pets-app-release"
			  		nexusArtifactUploader artifacts: [[artifactId: 'pets-app', classifier: '', file: 'target/pets-app.war', type: 'war']], 
					credentialsId: 'Nexus3', 
					groupId: 'in.Madhavi', 
					nexusUrl: '172.31.32.127:8081', 
					nexusVersion: 'nexus3', 
					protocol: 'http', 
					repository: nexusRepo, 
					version: version
				}
			}
		}
	}
}
