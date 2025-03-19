pipeline {
 agent any
 environment {
 CI_ENV = 'production'
 }
 stages {
 stage('Checkout') {
 steps {
 git branch: 'main', url:
'https://github.com/callmevan1120/CodeIgniter.git'
 }
 }
 stage('Install PHP & Composer') {
            steps {
                sh '''
                 # Ganti user menjadi root jika perlu
                if [ "$(id -u)" -ne 0 ]; then
                    echo "Switching to root..."
                    exec sudo -E bash "$0" "$@"
                fi
                # Perbarui package list
                apt update
                
                # Install PHP dan dependensi yang dibutuhkan
                apt install -y curl unzip php-cli php-mbstring php-xml php-curl php-bcmath php-tokenizer php-zip php-intl php-pdo php-mysql
                
                # Download dan install Composer jika belum ada
                if ! [ -x "$(command -v composer)" ]; then
                    curl -sS https://getcomposer.org/installer | php
                    mv composer.phar /usr/local/bin/composer
                fi
                '''
            }
        }
 stage('Install Dependencies') {
 steps {
 sh 'composer install --no-dev --optimize-autoloader'
 }
 }
 stage('Run Tests') {
 steps {
 sh 'phpunit'
 }
 post {
 success {
 junit 'application/tests/results/*.xml'
 }
 failure {
 echo 'Tests failed!'
 }
 }
 }
 stage('Deploy') {
 steps {
 echo 'Deploying to production environment...'
 // Tambahkan perintah deploy sesuai kebutuhan Anda
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