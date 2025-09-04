pipeline {
    agent any

    environment {
        // зміни під свою VM
        REMOTE_USER = 'stas'                 // твій користувач VM
        REMOTE_HOST = '127.0.0.1'
        REMOTE_PORT = '2222'       
        SSH_KEY = 'C:\\jenkins_key\\jenkins_key'  // шлях до приватного ключа
    }

    stages {
        stage('Install Apache2') {
            steps {
                echo 'Installing Apache on remote VM...'
                bat """
                  ssh -i %SSH_KEY% -o StrictHostKeyChecking=no %REMOTE_USER%@%REMOTE_HOST% "sudo apt update && sudo apt install -y apache2"
                """
            }
        }

        stage('Verify Apache') {
            steps {
                echo 'Checking if Apache is running...'
                bat """
                  ssh -i %SSH_KEY% -o StrictHostKeyChecking=no %REMOTE_USER%@%REMOTE_HOST% "systemctl status apache2 | grep Active"
                """
            }
        }

        stage('Check Logs for 4xx/5xx') {
            steps {
                echo 'Checking Apache logs for errors...'
                bat """
                  ssh -i %SSH_KEY% -o StrictHostKeyChecking=no %REMOTE_USER%@%REMOTE_HOST% "grep -E ' 4[0-9]{2} | 5[0-9]{2} ' /var/log/apache2/access.log || echo 'No 4xx/5xx errors found'"
                """
            }
        }
    }
}
