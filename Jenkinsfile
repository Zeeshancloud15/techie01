```groovy
pipeline {

    agent any

    environment {
        PATH = "/opt/maven/bin:${env.PATH}"

        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "100.62.106.141:8081"
        NEXUS_REPOSITORY = "maven-release"
        NEXUS_CREDENTIAL_ID = "nexus-creid"
    }

    stages {

        stage('Check Java and Maven') {
            steps {
                sh 'java -version'
                sh 'mvn -version'
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
            echo 'Check the console output'
        }
    }
}
```
