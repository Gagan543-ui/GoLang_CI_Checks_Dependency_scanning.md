pipeline {
    agent any

    environment {
        VENV = "venv"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Gagan543-ui/GoLang_CI_Checks_Dependency_scanning.md.git'
            }
        }

        stage('Setup Python Environment') {
            steps {
                sh '''
                python3 -m venv $VENV
                . $VENV/bin/activate
                pip install --upgrade pip
                pip install ansible ansible-lint yamllint molecule
                '''
            }
        }

        stage('YAML Lint Check') {
            steps {
                sh '''
                . $VENV/bin/activate
                yamllint .
                '''
            }
        }

        stage('Ansible Lint Check') {
            steps {
                sh '''
                . $VENV/bin/activate
                ansible-lint
                '''
            }
        }

        stage('Syntax Check') {
            steps {
                sh '''
                . $VENV/bin/activate
                ansible-playbook site.yml --syntax-check -i inventory
                '''
            }
        }

        stage('Role Tests') {
            steps {
                sh '''
                . $VENV/bin/activate
                ansible-playbook site.yml -i inventory
                '''
            }
        }
    }

    post {
        success {
            echo 'CI Checks Passed ✅'
        }
        failure {
            echo 'CI Checks Failed ❌'
        }
        always {
            echo 'Pipeline Execution Completed'
        }
    }
}
