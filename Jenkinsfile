pipeline {
    agent {
        //docker {
        //    image 'maven:3.8.6-openjdk-18-slim'
        //    args '-v /root/.m2:/root/.m2'
        //}
        node {
            label 'linux'
        }
    }
    tools { 
        dotnetsdk 'dotnet core 2.2' 
    }    
    stages {
        stage('Build') {
            steps {
                script {
                    CI_ERROR = "Failed while building application"
                }                
                
                sh 'dotnet --info'
            }
        }
        stage('Test') {
            steps {
                script {
                    CI_ERROR = "Failed while running test"
                }                   
                //sh 'mvn test'
            }
            //post {
                //always {
                    //junit 'target/surefire-reports/*.xml'
                //}
            //}
        }
        stage('Deliver') {
            steps {
                script {
                    CI_ERROR = "Failed while deploying application"
                }
                //sh './jenkins/scripts/deliver.sh'
            }
        }       
    }

    post {
        always {
            sendSlackNotifcation()
        }
    }
}

def sendSlackNotifcation()
{
    if ( currentBuild.currentResult == "SUCCESS" ) {
        buildSummary = "Job: ${env.JOB_NAME}\n Status: *SUCCESS*\n Build Report: ${env.BUILD_URL}CI-Build-HTML-Report"
        slackSend color : "good", message: "${buildSummary}", channel: 'cma-app-builder'
    }
    else {
        buildSummary = "Job: ${env.JOB_NAME}\n Status: *FAILURE*\n Error description: *${CI_ERROR}* \nBuild Report :${env.BUILD_URL}CI-Build-HTML-Report"
        slackSend color : "danger", message: "${buildSummary}", channel: 'cma-app-builder'
    }
}
