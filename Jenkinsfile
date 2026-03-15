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
                ddependencyCheck(
    additionalArguments: '--project "TP-Jenkins" --scan . --format HTML --format XML --nvdApiKey dc3bd8bf-2bb4-4b0b-8a57-493a521f95d6 --enableExperimental',
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