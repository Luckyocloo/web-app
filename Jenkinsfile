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

        stage('uploads to Nexus'){
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'maven-web-application', classifier: '', file: '/var/lib/jenkins/workspace/jomacs-jenkinsfile/target/web-app.war', type: 'war']], credentialsId: 'nexusnew', groupId: 'com.mt', nexusUrl: '3.83.25.250:8081/repository/jomacs-webapp', nexusVersion: 'nexus3', protocol: 'http', repository: 'jomacs-webapp', version: '3.8.1-snapshot'
            }
        }
    }
}
