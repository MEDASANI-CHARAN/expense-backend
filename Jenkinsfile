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
    parameters {
            booleanParam(name: 'Deploy', defaultValue: false, description: 'Toggle this value')
    }
    environment {
        def appVersion = '' //variable declaration
        nexusUrl = 'jenkins-nexus.daws78s.xyz:8081'
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

        stage('Build') {
            steps {
                sh """
                   zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                   ls -ltr
                """
            }
        } 

         stage('Sonar Scan') {
            environment {
                scannerHome = tool 'sonarqube7.1'
            }
            steps {
               script {
                    withSonarQubeEnv('sonarqube7.1') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        } 

         stage('Nexus artifact upload') {
            steps {
                        script {
                            nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    //nexusUrl: 'jenkins-nexus.daws78s.xyz:8081',
                    nexusUrl: "${nexusUrl}",
                    groupId: 'com.expense',
                    version: "${appVersion}",
                    repository: "backend",
                    credentialsId: 'nexus-auth',
                    artifacts: [
                        [artifactId: "backend",
                        classifier: '',
                        file: "backend-" + "${appVersion}" + '.zip',
                        type: 'zip']
                     ]
                   )
               }
            }
        } 

        stage('Deploy') {
            when {
                expression {
                    params.deploy
                }
            }
            steps {
                script{
                    def params = [
                    string(name: 'appVersion', value: "${appVersion}")
                ]
                    // build job: 'backend-deployment', parameters: [string(name: 'appVersion', value: "${appVersion}")]
                     build job: 'backend-deployment', parameters: params, wait: false
                }
              }
            }
        }
    
        
    post { 
            always { 
                echo 'I will always say Hello again!'
                deleteDir()
            }
            success { 
                echo 'I will un when pipeline sucess'
            }
        }
    } 

