pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    environment {
        def appVersion = '' //declarative
        nexusUrl = 'nexus.jpaws10s.online:8081'
        username = 'prashanth3010'
        password = 'Bunny@3010'
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
        stage ('build zip'){
            steps{
                sh """
                zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                ls -ltr
                """
            }
        }    

        stage ('build docker'){
            steps{
                sh """
                docker login -u ${username} --password=${password}

                docker build -t ${username}/backend:${appVersion} .

                docker push ${username}/backend:${appVersion}
                """
            }
        }

        stage('Deploy'){
            steps{
                sh """
                    aws eks update-kubeconfig --region us-east-1 --name expense-1
                    cd helm
                    sed -i 's/IMAGE_VERSION/${appVersion}/g' values.yaml
                    helm install backend .
                """
            }
        }


        // stage('Nexus Artifact Upload'){
        //     steps{
        //         script{
        //             nexusArtifactUploader(
        //                 nexusVersion: 'nexus3',
        //                 protocol: 'http',
        //                 nexusUrl: "${nexusUrl}",
        //                 groupId: 'com.expense',
        //                 version: "${appVersion}",
        //                 repository: "backend",
        //                 credentialsId: 'nexus-auth',
        //                 artifacts: [
        //                     [artifactId: "backend" ,
        //                     classifier: '',
        //                     file: "backend-" + "${appVersion}" + '.zip',
        //                     type: 'zip']
        //                 ]
        //             )
        //         }
        //     }
        // }
        // stage('deploy'){
        //     steps{
        //         script{
        //             def params= [
        //             string(name: 'appVersion', value: "${appVersion}")
        //         ]    
        //             build job: 'backend-deploy', parameters: params, wait:false
        //         }
        //     }
        // }
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
