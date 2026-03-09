// pipeline {
//     agent any
//     stages {
//         stage('Clone Repository') {
//             steps {
//                 git 'https://github.com/isianioui/tp-Jenkins'
//             }
//         }
//         stage('Install Dependencies') {
//             steps {
//                 sh 'pip install -r requirements.txt'
//             }
//         }
//         stage('Run Tests') {
//             steps {
//                 sh 'pytest'
//             }
//         }
//         stage('SAST Scan') {
//             steps {
//                 sh 'sonar-scanner' // Ensure SonarQube is configured in Manage Jenkins
//             }
//         }
//         stage('SCA Scan') {
//             steps {
//                 // --failOnCVSS 7 ensures the build fails if a vuln score >= 7 is found
//                 sh 'dependency-check.sh --project "TP-Jenkins" --scan . --format HTML --failOnCVSS 7'
//             }
//         }
//     }
//     post {
//         failure {
//             echo 'Build failed due to errors or vulnerabilities'
//         }
//     }
// }


pipeline {
    agent any
    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pip3 install --break-system-packages -r requirements.txt'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'pytest test_app.py'
            }
        }
        stage('SAST Scan') {
            steps {
                echo 'Skipping SAST for now...'
                // Once SonarQube is ready, you'll use:
                 tool 'sonar-scanner'
            }
        }
        stage('SCA Scan') {
            steps {
                // Use the plugin command instead of 'sh'
                // 'odcInstallation' must match the name 'DP-Check' you gave in Tools
                dependencyCheck additionalArguments: '--scan . --format HTML --format XML --failOnCVSS 7', odcInstallation: 'DP-Check'
            }
        }
        stage('Publish Reports') {
            steps {
                // This makes the report visible on the Jenkins project page
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
    }
    post {
        failure {
            echo 'Build failed due to errors or vulnerabilities'
        }
    }
}