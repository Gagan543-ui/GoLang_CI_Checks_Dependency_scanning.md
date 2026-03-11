pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/Gagan543-ui/GoLang_CI_Checks_Dependency_scanning.md.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                pip3 install ansible ansible-lint yamllint molecule
                '''
            }
        }

        stage('YAML Lint Check') {
            steps {
                sh 'yamllint .'
            }
        }

        stage('Ansible Lint Check') {
            steps {
                sh 'ansible-lint'
            }
        }

        stage('Syntax Check') {
            steps {
                sh 'ansible-playbook site.yml --syntax-check -i inventory'
            }
        }

        stage('Role Tests') {
            steps {
                sh 'ansible-playbook site.yml -i inventory'
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
    }
}
