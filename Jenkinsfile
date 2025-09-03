pipeline {
    agent any

    environment {
        REMOTE_HOST = "user@remote_vm_ip"  // IP або hostname твоєї VM
        SSH_KEY_ID = "jenkins_ssh_key"     // SSH Key у Jenkins Credentials
    }

    stages {
        stage('Install Apache2') {
            steps {
                echo "Installing Apache..."
                sshagent([env.SSH_KEY_ID]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $REMOTE_HOST << EOF
                        sudo apt update -y
                        sudo apt install -y apache2
                        sudo systemctl enable apache2
                        sudo systemctl start apache2
                        EOF
                    """
                }
            }
        }

        stage('Verify Apache') {
            steps {
                sshagent([env.SSH_KEY_ID]) {
                    sh """
                        ssh $REMOTE_HOST "apache2 -v || httpd -v"
                    """
                }
            }
        }

        stage('Check Logs for 4xx/5xx') {
            steps {
                sshagent([env.SSH_KEY_ID]) {
                    sh """
                        ssh $REMOTE_HOST << EOF
                        LOG_FILE="/var/log/apache2/access.log"
                        echo "4xx Errors:"
                        grep "HTTP/1.[01]\" [4][0-9][0-9]" \$LOG_FILE || echo "No 4xx errors found."
                        echo "5xx Errors:"
                        grep "HTTP/1.[01]\" [5][0-9][0-9]" \$LOG_FILE || echo "No 5xx errors found."
                        EOF
                    """
                }
            }
        }
    }
}
