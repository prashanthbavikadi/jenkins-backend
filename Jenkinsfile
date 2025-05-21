pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    stages {
        stage ('read the version'){
            steps{
                script{
                    def packageJson = readJson file: 'package.json'
                    def appVersion = packageJson.version 
                    echo "application vesion: $appVersion" 
                }
            }
        }
        stage('insatll dependencies') {
            steps {
               sh """
               npm install
               ls -ltr 
               echo "application vesion: $appVersion"
               """
            }
        }   
    }
    post {
        always {
            echo 'I will always say Hello again!'
            deleteDir()
        }
        success {
            echo 'I will run when pipeline is success'
        }
        failure {
            echo 'I will run when pipeline is failure'
        }
    }
}    
