pipeline {

    agent any

    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "100.62.106.141:8081"
        NEXUS_REPOSITORY = "maven-release"
        NEXUS_CREDENTIAL_ID = "nexus-creid"
    }

    tools {
        maven 'Maven'
        jdk 'JDK21'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Zeeshancloud15/techie01.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Publish to Nexus') {
            steps {
                nexusArtifactUploader(
                    nexusVersion: NEXUS_VERSION,
                    protocol: NEXUS_PROTOCOL,
                    nexusUrl: NEXUS_URL,
                    groupId: 'com.javatpoint',
                    version: '1.0-SNAPSHOT',
                    repository: NEXUS_REPOSITORY,
                    credentialsId: NEXUS_CREDENTIAL_ID,
                    artifacts: [
                        [
                            artifactId: 'SimpleCustomerApp',
                            classifier: '',
                            file: 'target/SimpleCustomerApp-1.0-SNAPSHOT.war',
                            type: 'war'
                        ]
                    ]
                )
            }
        }
    }

    post {
        success {
            echo 'BUILD SUCCESSFUL'
            echo 'WAR uploaded to Nexus successfully'
        }

        failure {
            echo 'BUILD FAILED'
            echo 'Check the Jenkins console output'
        }
    }
}
