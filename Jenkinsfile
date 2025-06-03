pipeline {
    agent any

    tools {
        // Use the Name you configured in Global Tool Configuration
        nodejs 'YourNodeJSInstallationName' // e.g., 'NodeJS-18'
    }

    environment {
        NPM_TOKEN_CREDENTIAL_ID = 'your-jenkins-credential-id-for-npm-token'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Node.js (and npm) will now be available from the configured tool
                sh 'node --version' // Good to verify
                sh 'npm --version'  // Good to verify
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test'
            }
        }

        stage('Publish to NPM') {
            steps {
                echo 'Publishing to NPM...'
                withCredentials([string(credentialsId: NPM_TOKEN_CREDENTIAL_ID, variable: 'NPM_AUTH_TOKEN')]) {
                    script {
                        if (NPM_AUTH_TOKEN == null || NPM_AUTH_TOKEN.trim().isEmpty()) {
                            error "NPM_AUTH_TOKEN not found or is empty. Check Jenkins credential '${NPM_TOKEN_CREDENTIAL_ID}'."
                        }
                        echo "Successfully retrieved NPM_AUTH_TOKEN. Proceeding with npm publish."
                        sh 'echo "//registry.npmjs.org/:_authToken=${NPM_AUTH_TOKEN}" > .npmrc'
                        sh 'npm publish'
                        sh 'rm .npmrc'
                        echo "Successfully published to NPM."
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
            cleanWs()
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
