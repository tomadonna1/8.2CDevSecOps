pipeline {
    agent {
        docker {
            image 'tomadonna/devsecops'
            args '--entrypoint="" -u root'
        } 
    }

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')  // required for SonarCloud stage
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

        stage('SonarCloud Analysis') {
            steps {
                sh '''
                    curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
                    unzip -q sonar-scanner.zip -d sonar-scanner
                    chmod +x sonar-scanner/sonar-scanner-5.0.1.3006-linux/bin/sonar-scanner
                    sonar-scanner/sonar-scanner-5.0.1.3006-linux/bin/sonar-scanner \
                      -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }
    }
}
