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
 stage('Install Dependencies') {
 steps {
 sh 'composer install'
 }
 }
 stage('Run Tests') {
 steps {
        sh '''
        mkdir -p application/tests/results
        vendor/bin/phpunit --log-junit application/tests/results/phpunit.xml
        '''
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