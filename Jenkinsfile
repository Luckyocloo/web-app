pipeline {
    agent any

    tools{
        maven 'jomacs'
    }

    stages{
        stage('git checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/Luckyocloo/web-app.git'
            }
        }
        stage('maven build'){
            steps{
                sh 'mvn clean package'
            }
        }

        stage('code analysis'){
           environment {
               ScannerHome = tool 'sonar'
           }
           steps{
               script{
                   withSonarQubeEnv('sonar'){
                       sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=adwoa-webapp"
                   }
               }
           }
        }
    }
}
