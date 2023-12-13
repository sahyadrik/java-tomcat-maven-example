pipeline {
    agent any
    environment {
        // Define the common variable for artifactory_path
        artifactoryPath = "/media/sahyadrikotipalli/common/artifactory"
        tomcatPath = "etc/tomcat10/"  // Replace with your Tomcat installation path
        tomcatManagerUrl = "http://robot:tronavatar@localhost:9090/manager/text"  // Replace with your Tomcat Manager URL
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

                        echo "Now Archiving the Artifacts to ${targetPath}...."
                        archiveArtifacts artifacts: '**/*.war', fingerprint: true
                        stash name: 'war', includes: '**/*.war'
                        
                        // Optionally, you can publish to Artifactory or move to the desired location
                        // Example: sh "cp -r ${WORKSPACE}/*.war ${targetPath}"
                    }
                }
            }
        } //stage1

        stage('Deploy to Tomcat') {
            steps {
                script {
                    // Unstash the WAR file from the previous stage
                    unstash 'war'

                    // Deploy the WAR file to Tomcat using a manual shell script
                    sh '''
                        #!/bin/bash

                        # Replace placeholders with your actual values
                        TOMCAT_PATH="${tomcatPath}"
                        TOMCAT_MANAGER_URL="${tomcatManagerUrl}"
                        WAR_FILE="${WORKSPACE}/*.war"

                        # Deploy using Tomcat Manager API
                        curl --upload-file "${WAR_FILE}" "${TOMCAT_MANAGER_URL}?path=/contextPath&update=true"
                    '''
                }
            }
        }

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
    }
}
