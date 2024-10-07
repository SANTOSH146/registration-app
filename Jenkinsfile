pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean install -Dmaven.test.skip=true"
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'webapp/target/*.war', fingerprint: true

            }
        }

        stage('Deployment') {
            steps {
                deploy adapters: [tomcat10(credentialsId: 'TomcatCreds', url: 'http://98.83.118.82:8080/')], 
                       contextPath: 'app', 
                       war: 'target/*.war'
            }
        }

        stage('Notification') {
            steps {
                emailext(
                    subject: "Job Completed",
                    body: "Jenkins pipeline job for maven build job completed",
                    to: "sudhir0504@gmail.com"
                )
            }
        }
    }
}
