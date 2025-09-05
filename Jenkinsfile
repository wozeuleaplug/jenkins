pipeline {
    agent { label 'Ubuntu-Agent' }

    stages {
        stage('Update system') {
            steps {
                sh '''
                sudo apt-get update -y
                '''
            }
        }
        stage('Install Apache2') {
            steps {
                sh '''
                sudo apt-get install -y apache2
                sudo systemctl enable apache2
                sudo systemctl start apache2
                '''
            }
        }
        stage('Check Apache status') {
            steps {
                sh 'systemctl status apache2 | grep Active'
            }
        }
        stage('Check Apache logs for errors') {
            steps {
                sh '''
                echo "Checking Apache logs for 4xx and 5xx errors..."
                sudo grep "HTTP/1.1\" [45][0-9][0-9]" /var/log/apache2/access.log || echo "No errors found"
                '''
            }
        }
    }
}
