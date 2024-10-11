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
        stage('prod deployment'){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://44.212.7.93:8080'), tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://44.212.7.93:8080')], contextPath: null, war: 'target/web-app.war'
            }
        }
    }
    post {
        success {
            slackSend channel: 'jenkins-test', color: 'good', message: "Build successful: ${currentBuild.fullDisplayName}"
        }
        failure {
            slackSend channel: 'jenkins-test', color: 'danger', message: "Build failed: ${currentBuild.fullDisplayName}"
        }
        aborted {
            slackSend channel: 'jenkins-test', color: 'warning', message: "Build aborted: ${currentBuild.fullDisplayName}"
        }
    }
}

