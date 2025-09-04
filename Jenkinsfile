pipeline {
    agent any

    stages {
        stage('Install Apache2') {
            steps {
                echo "Installing Apache on remote VM..."
                bat """
                ssh -i C:\\jenkins_key\\jenkins_key -p 2222 -o StrictHostKeyChecking=no stas@127.0.0.1 "sudo apt update && sudo apt install -y apache2"
                """
            }
        }

        stage('Verify Apache') {
            steps {
                echo "Checking if Apache is running..."
                bat """
                ssh -i C:\\jenkins_key\\jenkins_key -p 2222 -o StrictHostKeyChecking=no stas@127.0.0.1 "systemctl status apache2 | grep active"
                """
            }
        }

        stage('Check Logs for 4xx/5xx') {
            steps {
                echo "Checking Apache logs..."
                bat """
                ssh -i C:\\jenkins_key\\jenkins_key -p 2222 -o StrictHostKeyChecking=no stas@127.0.0.1 "grep -E ' 4[0-9]{2} | 5[0-9]{2} ' /var/log/apache2/access.log || echo 'No 4xx/5xx errors found'"
                """
            }
        }
    }
}
