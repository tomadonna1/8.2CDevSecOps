pipeline {
    agent {
        docker {
            image 'tomadonna/devsecops'
            args '-u root'
        }
    }

    stages {
        stage('NPM Audit') {
            steps {
                sh 'npm audit || true'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }
        stage('Coverage') {
            steps {
                sh 'npm run coverage || true' // This will show known CVEs in the output
            }
        }
    }
}
