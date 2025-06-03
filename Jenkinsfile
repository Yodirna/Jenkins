pipeline {
    agent any

    environment {
        // Define the Jenkins credential ID for your npm token
        NPM_TOKEN_CREDENTIAL_ID = 'NPM'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building the project...'
                // Example build command (replace with your actual build command)
                sh 'npm install'
                sh 'npm run build' // Or your specific build script
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                // Example test command (replace with your actual test command)
                sh 'npm test'
            }
        }

        stage('Publish to NPM') {
            steps {
                echo 'Publishing to NPM...'
                // Use withCredentials to securely access the npm token
                withCredentials([string(credentialsId: NPM_TOKEN_CREDENTIAL_ID, variable: 'NPM_TOKEN')]) {
                    // Configure npm to use the token for authentication
                    // The //registry.npmjs.org/:_authToken part is important
                    sh 'echo "//registry.npmjs.org/:_authToken=${NPM_TOKEN}" > .npmrc'
                    sh 'npm publish'
                    // It's good practice to remove the .npmrc file after publishing
                    sh 'rm .npmrc'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
            // Clean up workspace or other post-build actions
            cleanWs()
        }
    }
}
