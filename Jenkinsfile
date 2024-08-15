pipeline {
    agent any

    environment {
        GIT_REPO_URL = 'https://github.com/Pratik-Pardeshi/newone.git'
        GIT_CREDENTIALS_ID = 'https://github.com/Pratik-Pardeshi/newone.git'
    }

    stages {
        stage('Checkout') {
            steps {
                // Clean up workspace and checkout the repository
                deleteDir() // Clean up the workspace before checking out the code
                checkout([$class: 'GitSCM',
                          userRemoteConfigs: [[url: "${env.GIT_REPO_URL}", credentialsId: "${env.GIT_CREDENTIALS_ID}"]],
                          branches: [[name: '*/main']]]) // Adjust the branch as needed
            }
        }

        stage('Prepare') {
            steps {
                script {
                    // Ensure that index.html is present
                    if (!fileExists('index.html')) {
                        error "index.html not found in repository."
                    }
                }
            }
        }

        stage('Send File and Deploy') {
            steps {
                script {
                    // Copy index.html to target server
                    sh 'scp -i /var/lib/jenkins/.ssh/id_rsa index.html ansible@172.31.36.135:/tmp/index.html'

                    // Run Ansible playbook
                    sh 'ssh -i /var/lib/jenkins/.ssh/id_rsa ansible@172.31.36.135 "ansible-playbook /home/ansible/playbooks/deploy.yml"'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
