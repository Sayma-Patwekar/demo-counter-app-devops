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

        stage('upload war file to nexus'){
            steps{
                script{
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
                    repository: 'demo-counter-app-release', 
                    version: '1.0.0'
                }
            }
        }
    }
}
