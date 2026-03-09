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

// pipeline {
//     agent any

//     stages {

//         stage('Install Dependencies') {
//             steps {
//                 sh 'pip3 install --break-system-packages -r requirements.txt'
//             }
//         }

//         stage('Run Tests') {
//             steps {
//                 sh 'pytest test_app.py'
//             }
//         }

//        stage('SAST Scan') {
//     steps {
//         script {
//             def scannerHome = tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
//             withSonarQubeEnv('sonar-server') {
//                 sh "${scannerHome}/bin/sonar-scanner"
//             }
//         }
//     }
// }

//         stage('SCA Scan') {
//             steps {
//                 dependencyCheck additionalArguments: '--scan . --format HTML --format XML --failOnCVSS 7', odcInstallation: 'DP-Check'
//             }
//         }

//         stage('Publish Reports') {
//             steps {
//                 dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
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

    tools {
        // Make sure Python and SonarScanner are configured in Jenkins tools
        python 'Python3'
        sonarScanner 'sonar-scanner'
    }

    stages {

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

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
                script {
                    def scannerHome = tool name: 'sonar-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('sonar-server') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }

        stage('SCA Scan') {
            steps {
                // Corrected command: -f for format, -o for output
                dependencyCheck additionalArguments: '--scan . -f HTML -f XML -o . --failOnCVSS 7', odcInstallation: 'DP-Check'
            }
        }

        stage('Publish Reports') {
            steps {
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
                // HTML publisher requires the HTML Publisher Plugin
                publishHTML target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: '.',
                    reportFiles: 'dependency-check-report.html',
                    reportName: 'OWASP Dependency-Check Report'
                ]
            }
        }
    }

    post {
        failure {
            echo 'Build failed due to errors or vulnerabilities'
        }
        success {
            echo 'Build completed successfully'
        }
    }
}