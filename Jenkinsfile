pipeline {
    agent any
    environment {
        // Define the common variable for artifactory_path
        artifactoryPath = "/media/sahyadrikotipalli/common/artifactory"
    }
    stages {
        stage('Build Application') {
            steps {
                sh 'mvn -f pom.xml clean package'
            }
            post {
                success {
                    script {
                        // Use Jenkins timestamp to create a unique directory for each build
                        def timestamp = new Date().format("yyyyMMdd_HHmmss")
                        def targetPath = "${artifactoryPath}/build_${timestamp}"

                        echo(message: '${WORKSPACE}')
                        echo "Now Archiving the Artifacts to ${targetPath}...."
                        archiveArtifacts artifacts: '**/*.war', fingerprint: true
                        stash name: 'war', includes: '**/*.war'
                        
                        // Optionally, you can publish to Artifactory or move to the desired location
                        // Example: sh "cp -r ${WORKSPACE}/*.war ${targetPath}"
                    }
                }
            }
        } //stage1
    }
}