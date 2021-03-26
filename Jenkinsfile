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
                    script {
                        def myImage = docker.build("train-schedule-app-vovanVersion:${env.BUILD_ID}") 

                        testImage.inside {
                        sh 'echo &(curl localhost:8080)'
                        }      
                    }
            }
        }
        stage('Push Docker Image') {
            when {
                branch 'master'
            }
                steps {
                    script {
                        docker.withRegistry('https://registry.example.com', 'credentials-id') {
                            customImage.push()
                        }
                    }
            }
        }
        stage('DeployToProduction') {
            when {
                branch 'master'
            }
            steps{
                input 'Deploy to production?'
                milestone(1)
                    script {
                        def remote = [:]
                        remote.host = $prod_ip
                        remote.allowAnyHosts = true

                        withCredentials([sshUserPrivateKey(credentialsId: 'sshUser', passphraseVariable: 'userPass', usernameVariable: 'userName')]) {
                        remote.user = userName
                        remote.password = userPass
                        remote.identityFile = identity
                        sshCommand remote: remote, command: 'for i in {1..5}; do echo -n \"Loop \$i \"; date ; sleep 1; done'
                        }      
                    }
            }
        }
    }
}
