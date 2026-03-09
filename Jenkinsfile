pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest test_app.py -v'
            }
        }

        stage('SCA Scan - OWASP Dependency Check') {
            steps {
                dependencyCheck(
                    additionalArguments: '--project "TP-Jenkins" --scan . --format HTML --failOnCVSS 7',
                    odcInstallation: 'OWASP-DC'
                )
            }
            post {
                always {
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                }
            }
        }
    }

    post {
        failure {
            echo 'Build FAILED — vulnerabilities or test errors detected!'
        }
        success {
            echo 'Build PASSED — no critical vulnerabilities found.'
        }
    }
}