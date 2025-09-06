pipeline {
    agent { label 'Ubuntu-Agent' }

    stages {
        stage('Update system') {
            steps {
                sh '''
                echo "=== Updating system ==="
                sudo apt-get update -y
                '''
            }
        }

        stage('Install Apache2') {
            steps {
                sh '''
                echo "=== Installing Apache2 ==="
                sudo apt-get install -y apache2
                sudo systemctl enable apache2
                sudo systemctl start apache2
                '''
            }
        }

        stage('Check Apache status') {
            steps {
                sh '''
                echo "=== Checking Apache status ==="
                sudo systemctl status apache2 --no-pager
                echo "=== Checking local server response ==="
                curl -I http://localhost | head -n 5
                '''
            }
        }

        stage('Check Apache Logs for errors') {
            steps {
                sh '''
                echo "=== Checking access.log for 4xx and 5xx ==="
                sudo grep "HTTP/1.1\\\\\" [45][0-9][0-9]" /var/log/apache2/access.log || echo "No errors found"

                echo "=== Last 20 lines of error.log ==="
                sudo tail -n 20 /var/log/apache2/error.log
                '''
            }
        }
    }
}
