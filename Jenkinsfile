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
                sh '''
                composer install --no-dev --optimize-autoloader

                # Gunakan perintah sed yang baru dengan backup (.bak)
                FILE="vendor/mikey179/vfsstream/src/main/php/org/bovigo/vfs/vfsStream.php"
                if [ -f "$FILE" ]; then
                    sed -i.bak s/name{0}/name[0]/ "$FILE"
                else
                    echo "File $FILE tidak ditemukan, melewati perintah sed."
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
