pipeline {
    agent any

    environment {
        APACHE_LOG = "/var/log/apache2/access.log"
    }

    stages {
        stage('Install Apache2') {
            steps {
                script {
                    sh '''
                        echo "=== Updating system packages ==="
                        sudo apt-get update -y

                        echo "=== Installing Apache2 ==="
                        sudo apt-get install -y apache2

                        echo "=== Enabling and starting Apache2 service ==="
                        sudo systemctl enable apache2
                        sudo systemctl start apache2
                    '''
                }
            }
        }

        stage('Verify Apache2 Status') {
            steps {
                script {
                    sh '''
                        echo "=== Checking Apache2 status ==="
                        if systemctl is-active --quiet apache2; then
                            echo "Apache2 is running ✅"
                        else
                            echo "Apache2 failed to start ❌"
                            exit 1
                        fi
                    '''
                }
            }
        }

        stage('Check Apache2 Logs for Errors') {
            steps {
                script {
                    sh '''
                        echo "=== Checking Apache logs for 4xx/5xx errors ==="
                        if sudo grep -E "HTTP/1.[01]\" [45][0-9]{2}" $APACHE_LOG; then
                            echo "❌ Found HTTP errors (4xx/5xx) in logs!"
                            exit 1
                        else
                            echo "✅ No 4xx/5xx errors found in logs."
                        fi
                    '''
                }
            }
        }

        stage('Archive Logs') {
            steps {
                script {
                    sh '''
                        echo "=== Copying Apache log file ==="
                        sudo cp $APACHE_LOG apache_access.log || true
                    '''
                }
                archiveArtifacts artifacts: 'apache_access.log', followSymlinks: false
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully. Apache2 installed and logs checked."
        }
        failure {
            echo "❌ Pipeline failed. Check logs in Jenkins console output."
        }
    }
}
