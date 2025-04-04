pipeline {
    agent {
        label 'agent-1'
    }
    options {
                // timeout(time: 100, unit: 'SECONDS')
                timeout(time: 5, unit: 'MINUTES')
                disableConcurrentBuilds() 
                 ansiColor('xterm')
            }
    environment {
        def appVersion = '' //variable declaration
    }
    stages {
        stage('read the version') {
            steps {
                script {
                def packageJson = readJSON file: 'package.json'
                //def appVersion = packageJson.version
                appVersion = packageJson.version
                echo "application version: $appVersion"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh """
                    npm install
                    ls -ltr
                    echo $appVersion
                """
            }
        } 

    }
        
    post { 
            always { 
                echo 'I will always say Hello again!'
                //deleteDir()
            }
            success { 
                echo 'I will un when pipeline sucess'
            }
        }
    }
