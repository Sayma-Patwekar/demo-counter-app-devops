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
                sh 'mvn test'
            }
        }
    }
}
