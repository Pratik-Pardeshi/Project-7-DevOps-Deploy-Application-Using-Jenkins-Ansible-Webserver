pipeline {
    agent any
    environment {
        SSH_KEY = credentials('ansible-ssh-key')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Pratik-Pardeshi/Project-7-DevOps-Deploy-Application-Using-Jenkins-Ansible-Webserver.git'
            }
        }
        stage('Send File to Ansible Server') {
            steps {
                sh '''
                scp -o StrictHostKeyChecking=no -i $SSH_KEY index.html ansible@172.31.45.77:/tmp/index.html
                '''
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                sh '''
                ssh -o StrictHostKeyChecking=no -i $SSH_KEY ansible@172.31.45.77 'ansible-playbook /home/ansible/playbooks/deploy.yml'
                '''
            }
        }
    }
    post {
        failure {
            echo 'Pipeline failed.'
        }
        success {
            echo 'Pipeline completed successfully.'
        }
    }
}
