pipeline {
    agent none

    stages {
        stage('NPM Commands') {
            agent {
                docker {
                    image 'node:17-bullseye'
                    args '-v /tmp:/tmp' // Mount /tmp for shared volume
                }
            }

            environment {
                XDG_DATA_HOME = "/tmp"
            }

            steps {
                script {
                    checkout scm

                    // Build TypeScript code and run TypeScript tests
                    dir('./DotnetTemplate.Web') {
                        sh 'npm install'
                        sh 'npm run build'
                        sh 'npm test'
                        sh 'npm run lint'
                    }
                }
            }
        }

        stage('Dotnet Commands') {
            agent {
                docker {
                    image 'mcr.microsoft.com/dotnet/sdk:7.0'
                    args '-v /tmp:/tmp' // Mount /tmp for shared volume
                }
            }

            environment {
                DOTNET_CLI_HOME = "/tmp/dotnet_cli_home"
                XDG_DATA_HOME = "/tmp"
            }

            steps {
                script {
                    checkout scm

                    // Build Csharp code and run Csharp tests
                    sh 'dotnet build'
                    sh 'dotnet test'
                }
            }
        }
    }
}