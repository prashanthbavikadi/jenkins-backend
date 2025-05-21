pipeline {
    agent {
        label 'AGENT-1'
    }
    // options {
    //     timeout(time: 30, unit: 'MINUTES')
    //     disableConcurrentBuilds()
    //     ansiColor('xterm')
    // }
    environment {
        def appVersion = '' //decalartaive 
    }
    stages {
        stage('read the version'){
            steps{
                script{
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version 
                    echo "application vesion: $appVersion" 
                }
            }
        }
        stage('insatll dependencies') {
            steps{
               sh """
                npm install
                ls -ltr 
                echo "application vesion: $appVersion"
               """
            }
        } 
        stage ('build'){
            steps{
                sh """
                zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                ls -ltr
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
