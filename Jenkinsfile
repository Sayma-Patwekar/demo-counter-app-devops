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
    }
}
