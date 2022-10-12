pipeline {
	agent any
	stages {
		stage('Maven: Build Application') {
			sh 'mvn clean package'
			}
			post {
				success {
					echo "Now Archiving the Artifacts...."
					archiveArtifacts artifacts:'**/*.war'
				}
			}
		}
	}
}
	