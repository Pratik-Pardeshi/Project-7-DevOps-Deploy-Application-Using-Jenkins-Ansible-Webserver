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
                withCredentials([sshUserPrivateKey(credentialsId: 'ansible-ssh-key', keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                    scp -o StrictHostKeyChecking=no -i $SSH_KEY $WORKSPACE/index.html ansible@172.31.15.242:/tmp/index.html
                    '''
                }
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ansible', keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no -i $SSH_KEY ansible@172.31.36.31 'ansible-playbook -vvv /home/ansible/playbooks/deploy.yml'
                    '''
                }
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
