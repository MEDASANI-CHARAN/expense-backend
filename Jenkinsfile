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
    stages {
        stage('test') {
            steps {
                sh '''
                    echo "this is testng"
                '''
            }
        } 
        
    post { 
            always { 
                echo 'I will always say Hello again!'
                deleteDir()
            }
            success { 
                echo 'I will when when pipeline sucess'
            }
        }
    }