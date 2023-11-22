pipeline {
    agent any

    stages {
        stage('Build and test') {
            steps {
                script {
                    checkout scm

                    // Build Csharp code
                    sh 'dotnet build'

                    // Build TypeScript code
                    dir('./DotnetTemplate.Web') {
                        sh 'npm install'
                        sh 'npm run build'
                    }

                    // Run Csharp tests
                    sh 'dotnet test'

                    // Run TypeScript tests
                    dir('./DotnetTemplate.Web') {
                        sh 'npm t'
                    }

                    // Run TypeScript Linter
                    dir('./DotnetTemplate.Web') {
                        sh 'npm run lint'
                    }
                }
            }
        }
    }
}
