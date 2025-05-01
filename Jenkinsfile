pipeline {
    agent {
        docker {
            image 'tomadonna/devsecops'
            args '--entrypoint="" -u root'
        }
    }

    stages {
        stage('NPM Audit') {
            steps {
                sh 'npm audit || true' // This will show known CVEs in the output
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || true' // Allows pipeline to continue despite test failures
            }
        }
        stage('Generate Coverage Report') {
            steps {
                // Ensure coverage report exists
                sh 'npm run coverage || true' 
            }
        }
    }
}
