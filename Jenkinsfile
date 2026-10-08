pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out project from GitHub...'
                checkout scm
            }
        }

        stage('Verify Tools') {
            steps {
                sh 'ansible --version'
                sh 'docker --version'
                sh 'docker ps'
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh '''
                    ansible-playbook \
                    -i inventory \
                    playbook.yml
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'docker ps'
                sh 'curl -I http://host.docker.internal:8082'
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}
