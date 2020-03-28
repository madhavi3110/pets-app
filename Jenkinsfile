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
		stage('Deploy-Tomcat'){
			steps{
				sshagent(['tomcat-dev']) {
    				// copy war file to tomcat webapps
					sh "scp -o StrictHostKeyChecking=no target/*.war ec2-user@172.31.42.210:/opt/tomcat8/webapps/pets-app.war"
					//stop and start tomcat
					sh "ssh ec2-user@172.31.42.210:/opt/tomcat8/bin/shutdown.sh"
					sh "ssh ec2-user@172.31.42.210:/opt/tomcat8/bin/startup.sh"
				}
			}
		}
	}
}
