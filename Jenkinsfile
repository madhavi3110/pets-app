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
				script{
				sshagent(['tomcat-dev']) {
					def userHost = "ec2-user@172.31.42.210"
					def tomcatBin = "ec2-user@172.31.42.210 /opt/tomcat8/bin"
    				// copy war file to tomcat webapps
					sh "scp -o StrictHostKeyChecking=no target/*.war ${userHost}:/opt/tomcat8/webapps/pets-app.war"
					//stop and start tomcat
					sh "ssh ${tomcatBin}/shutdown.sh"
					sh "ssh ${tomcatBin}/startup.sh"
				    }
				}
			}
		}
	}
}
