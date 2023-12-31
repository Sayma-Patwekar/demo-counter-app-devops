pipeline{

    agent any

    stages{

        stage('Git Checkout'){
            steps{
                git 'https://github.com/Sayma-Patwekar/demo-counter-app-devops.git'
            }
        }

        stage('Unit Testing'){
            steps{
                bat 'mvn test'
            }
        }

        stage('Integration Testing'){
            steps{
                bat 'mvn verify -DskipUnitTests'
            }
        }

        stage('Maven Build'){
            steps{
                bat 'mvn clean install'
            }
        }

        stage('Static code analysis'){
            steps{
                withSonarQubeEnv(credentialsId: 'sonarqube-api-key', installationName: 'sonarqube') {
                 bat 'mvn clean package sonar:sonar'
                }
            }
        }

        stage('Quality Gate Status'){
            steps{
                waitForQualityGate abortPipeline: false, credentialsId: 'sonarqube-api-key'
            }
        }

        stage('upload jar file to nexus'){
            steps{
                script{

                    def readPomVersion = readMavenPom file: 'pom.xml'

                    def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "demo-counter-app-snapshot" : "demo-counter-app-release"

                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: 'springboot', 
                            classifier: '', 
                            file: 'target/Uber.jar', 
                            type: 'jar'
                        ]
                    ], 
                    credentialsId: 'nexus-auth', 
                    groupId: 'com.example', 
                    nexusUrl: 'localhost:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: nexusRepo, 
                    version: "${readPomVersion.version}"
                }
            }
        }

    }
}
