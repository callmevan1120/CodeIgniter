pipeline {
    agent any

    environment {
        CI_ENV = 'production'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/username/CodeIgniter.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                composer install --no-dev --optimize-autoloader
                
                # Cek apakah file vfsStream.php ada sebelum menjalankan sed
                if [ -f vendor/mikey179/vfsstream/src/main/php/org/bovigo/vfs/vfsStream.php ]; then
                    sed -i s/name{0}/name[0]/ vendor/mikey179/vfsstream/src/main/php/org/bovigo/vfs/vfsStream.php
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
