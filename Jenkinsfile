pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/your-username/your-repo.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'pytest'
            }
        }
        stage('SAST Scan') {
            steps {
                sh 'sonar-scanner' // Ensure SonarQube is configured in Manage Jenkins
            }
        }
        stage('SCA Scan') {
            steps {
                // --failOnCVSS 7 ensures the build fails if a vuln score >= 7 is found
                sh 'dependency-check.sh --project "TP-Jenkins" --scan . --format HTML --failOnCVSS 7'
            }
        }
    }
    post {
        failure {
            echo 'Build failed due to errors or vulnerabilities'
        }
    }
}