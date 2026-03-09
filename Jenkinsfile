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
        // We removed the 'Clone Repository' stage because Jenkins does it automatically!
        
        stage('Install Dependencies') {
            steps {
                // Using pip3 ensures it uses Python 3
                sh 'pip3 install -r requirements.txt'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'pytest test_app.py'
            }
        }
        stage('SAST Scan') {
            steps {
                // If you haven't installed SonarQube yet, comment this stage out
                // sh 'sonar-scanner' 
                echo 'Skipping SAST for now...'
            }
        }
        stage('SCA Scan') {
            steps {
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