pipeline {
    agent any

    environment {
        GO_HOME = "/usr/local/go"
        PATH = "${GO_HOME}/bin:${env.PATH}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Gagan543-ui/GoLang_CI_Checks_Dependency_scanning.md.git'
            }
        }

        stage('Check Go Version') {
            steps {
                sh 'which go'
                sh 'go version'
            }
        }

        stage('Clean Go Cache') {
            steps {
                sh 'rm -rf $HOME/go || true'
                sh 'go clean -cache -modcache'
            }
        }

        stage('Verify Dependencies') {
            steps {
                sh 'go mod tidy'
            }
        }

        stage('Build') {
            steps {
                sh 'go build ./...'
            }
        }

        stage('Test') {
            steps {
                sh '''
                go install github.com/jstemmer/go-junit-report@latest
                go test -v ./... 2>&1 | $HOME/go/bin/go-junit-report > test-report.xml
                '''
            }
        }

        stage('Dependency Scan') {
            steps {
                sh '''
                go install golang.org/x/vuln/cmd/govulncheck@latest
                $HOME/go/bin/govulncheck -json ./... > vuln-report.json
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '*.xml, *.json', fingerprint: true
            junit 'test-report.xml'
        }

        success {
            slackSend message: "Build SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        }

        failure {
            slackSend message: "Build FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
        }
    }
}
