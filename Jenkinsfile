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

        stage('Debug') {
            steps {
                sh 'ls -la webapp/target'
            }
        }


        stage('Deployment') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcatserverCred', url: 'http://98.83.118.82:8080/')], 
                       contextPath: 'app', 
                       war: 'webapp/target/*.war'
            }
        }

        stage('Notification') {
            steps {
                emailext(
                    subject: "Job Completed",
                    body: "Jenkins pipeline job for maven build job completed",
                    to: "santoshk6r@gmail.com"
                )
            }
        }
    }
}
