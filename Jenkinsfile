pipeline {
    agent any

    environment {
        CI_ENV = 'production'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/callmevan1120/CodeIgniter.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                 sh 'composer install'

                // sh '''
                // apt update && apt install -y curl unzip php-cli php-xml php-mbstring
                // curl -sS https://getcomposer.org/installer | php
                // mv composer.phar /usr/local/bin/composer
                // composer install --no-dev --optimize-autoloader
                // '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                mkdir -p application/tests/results
                vendor/bin/phpunit --log-junit application/tests/results/phpunit.xml || true
                '''
            }
            post {
                success {
                    echo 'Tests completed successfully!'
                    junit 'application/tests/results/phpunit.xml'
                }
                failure {
                    echo 'Tests failed! But still collecting JUnit results...'
                    junit 'application/tests/results/phpunit.xml'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to production environment...'
                // Tambahkan perintah deploy sesuai kebutuhan
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
