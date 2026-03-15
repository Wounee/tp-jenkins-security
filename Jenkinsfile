pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                bat 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'pytest test_app.py -v'
            }
        }

        stage('SCA Scan - OWASP Dependency Check') {
            steps {
                dependencyCheck(
                    additionalArguments: '--project "TP-Jenkins" --scan . --format HTML --format XML',
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