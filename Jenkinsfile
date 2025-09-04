pipeline {
    agent any

    stages {
        stage('Install Apache2') {
            steps {
                script {
                    // Для Ubuntu/Debian
                    sh '''
                    sudo apt update
                    sudo apt install -y apache2
                    sudo systemctl start apache2
                    sudo systemctl enable apache2
                    '''
                    
                }
            }
        }
        stage('Check Apache2 Status') {
            steps {
                sh 'systemctl status apache2 || systemctl status httpd'
            }
        }
        stage('Read Logs and Check Errors') {
            steps {
                sh '''
                # Витяг 4xx і 5xx з логів
                if [ -f /var/log/apache2/access.log ]; then
                    echo "Errors in Apache2 logs:"
                    grep " 4[0-9][0-9] " /var/log/apache2/access.log
                    grep " 5[0-9][0-9] " /var/log/apache2/access.log
                elif [ -f /var/log/httpd/access_log ]; then
                    echo "Errors in httpd logs:"
                    grep " 4[0-9][0-9] " /var/log/httpd/access_log
                    grep " 5[0-9][0-9] " /var/log/httpd/access_log
                else
                    echo "No log file found"
                fi
                '''
            }
        }
    }
}

