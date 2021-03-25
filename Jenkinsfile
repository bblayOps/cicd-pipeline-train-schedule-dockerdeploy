pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            when {
                branch 'master'
            }
                steps {
                    ""
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
                steps {
                    ""
            }
        }
        stage('DeployToProduction') {
            when {
                branch 'master'
            }
                steps {
                    ""
            }
        }
    }
}
