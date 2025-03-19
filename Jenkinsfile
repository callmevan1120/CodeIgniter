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
        sh '''
        # Install dependencies dengan Composer
        composer install --no-dev --optimize-autoloader

        # Periksa apakah file vfsStream.php ada sebelum menjalankan sed
        VFS_FILE="vendor/mikey179/vfsstream/src/main/php/org/bovigo/vfs/vfsStream.php"
        if [ -f "$VFS_FILE" ]; then
            sed -i 's/name{0}/name[0]/' "$VFS_FILE"
            echo "Patch berhasil diterapkan pada $VFS_FILE"
        else
            echo "File $VFS_FILE tidak ditemukan, melewati patch..."
        fi
        '''
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