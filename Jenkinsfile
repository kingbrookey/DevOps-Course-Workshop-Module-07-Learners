pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/dotnet/sdk:7.0'
        }
    }

    environment {
        DOTNET_CLI_HOME = "/tmp/dotnet_cli_home"
        XDG_DATA_HOME = "/tmp"
    }

    stages {
        stage('Build and test') {
            steps {
                script {
                    checkout scm

                    // Build Csharp code
                    sh 'dotnet build'

                    // Build TypeScript code and run TypeScript tests
                    dir('./DotnetTemplate.Web') {
                        sh 'npm install'
                        sh 'npm run build'
                        sh 'npm test'
                        sh 'npm run lint'
                    }

                    // Run Csharp tests
                    sh 'dotnet test'
                }
            }
        }
    }
}