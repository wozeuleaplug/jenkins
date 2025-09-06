pipeline {
    agent { label 'Ubuntu-Agent' }

    stages {
        stage('Update system') {
            steps {
                sh '''
                echo "=== Оновлення системи ==="
                sudo apt-get update -y
                '''
            }
        }

        stage('Install Apache2') {
            steps {
                sh '''
                echo "=== Встановлюємо Apache2 ==="
                sudo apt-get install -y apache2
                sudo systemctl enable apache2
                sudo systemctl start apache2
                '''
            }
        }

        stage('Check Apache status') {
            steps {
                sh '''
                echo "=== Перевіряємо статус Apache ==="
                sudo systemctl status apache2 --no-pager
                echo "=== Перевіряємо відповідь від локального сервера ==="
                curl -I http://localhost | head -n 5
                '''
            }
        }

        stage('Check Apache Logs for errors') {
            steps {
                sh '''
                echo "=== Перевірка access.log на 4xx та 5xx ==="
                sudo grep "HTTP/1.1\\\" [45][0-9][0-9]" /var/log/apache2/access.log || echo "Помилок не знайдено"

                echo "=== Останні 20 рядків error.log ==="
                sudo tail -n 20 /var/log/apache2/error.log
                '''
            }
        }
    }
}
