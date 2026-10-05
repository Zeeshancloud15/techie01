pipeline {
    agent {
        label "master"
    }

    tools {
        maven "maven-integration"
    }

    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "100.62.106.141:8081"
        NEXUS_REPOSITORY = "maven-release"
        NEXUS_CREDENTIAL_ID = "nexus-creid"
    }

    stages {

        stage("Clone Code") {
            steps {
                git 'https://github.com/Zeeshancloud15/techie01.git'
            }
        }

        stage("Maven Build") {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true install'
            }
        }

        stage("Publish to Nexus") {
            steps {
                script {
                    def pom = readMavenPom file: "pom.xml"

                    def filesByGlob = findFiles(
                        glob: "target/*.${pom.packaging}"
                    )

                    if (filesByGlob.length == 0) {
                        error "No artifact found in target directory"
                    }

                    def artifactPath = filesByGlob[0].path

                    echo "Artifact: ${filesByGlob[0].name}"
                    echo "Path: ${artifactPath}"
                    echo "Group ID: ${pom.groupId}"
                    echo "Artifact ID: ${pom.artifactId}"
                    echo "Version: ${pom.version}"
                    echo "Packaging: ${pom.packaging}"

                    if (fileExists(artifactPath)) {
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [
                                    artifactId: pom.artifactId,
                                    classifier: '',
                                    file: artifactPath,
                                    type: pom.packaging
                                ],
                                [
                                    artifactId: pom.artifactId,
                                    classifier: '',
                                    file: "pom.xml",
                                    type: "pom"
                                ]
                            ]
                        )
                    } else {
                        error "Artifact ${artifactPath} could not be found"
                    }
                }
            }
        }
    }
}
