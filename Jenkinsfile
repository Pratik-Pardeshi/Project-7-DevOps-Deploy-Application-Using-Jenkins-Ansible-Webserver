pipeline {
    agent any
    environment {
        SSH_KEY = credentials('ansible-ssh-key') // Use the ID you provided in Jenkins
    }
    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/Pratik-Pardeshi/newone.git', branch: 'main'
            }
        }
        stage('Move index.html to Ansible Server') {
            steps {
                script {
                    sh """
                        scp -i ${SSH_KEY} index.html ansible@172.31.34.36:/tmp/index.html
                    """
                }
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                script {
                    sh """
                        ssh -i ${SSH_KEY} ansible@172.31.34.36 'ansible-playbook /home/ansible/playbooks/deploy.yml'
                    """
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
