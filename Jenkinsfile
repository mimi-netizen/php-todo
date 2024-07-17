pipeline {
    agent any

    stages {
        stage("Initial cleanup") {
            steps {
                dir("${WORKSPACE}") {
                    deleteDir()
                }
            }
        }

        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/mimi-netizen/php-todo.git'
            }
        }

        stage('Prepare Dependencies') {
            steps {
                sh 'mkdir -p bootstrap/cache'
                sh 'composer install'
            }
        }

        // Add a stage for deployment or provisioning
        stage('Deploy') {
            steps {
                sh 'php artisan migrate'
                sh 'php artisan db:seed'
                sh 'php artisan key:generate'
            }
        }

        stage('Execute Unit Tests') {
            steps {
                sh './vendor/bin/phpunit'
            }
        }

        stage('Code Analysis') {
            steps {
                 sh 'phploc app/ --log-csv build/logs/phploc.csv'

            }
        }

    }
}