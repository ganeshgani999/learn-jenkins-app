pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "--- BUILDING ---"
                    npm ci
                    npm run build
                '''
                // Stash the build artifacts to use in the next stage
                stash name: 'build-artifacts', includes: 'build/**'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                // Unstash the artifacts from the 'Build' stage
                unstash 'build-artifacts'
                
                sh '''
                    echo "--- TESTING ---"
                    test -f build/index.html
                    npm test
                '''
            }
        }
    }
    post {
        always {
            // Corrected typo: 'test-results'
            junit 'test-results/junit.xml'
        }
    }
}
