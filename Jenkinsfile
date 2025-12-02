pipeline {
    agent any

    environment {
        DEPLOY_DIR = "C:\\mini-site-deploy"
        DIST_DIR   = "dist"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Recuperation du code depuis GitHub..."
                git branch: 'main', url: 'https://github.com/dhahri372/mini-site-jenkins.git'
            }
        }

        stage('Build') {
            steps {
                echo "Build du projet (minification CSS/JS)..."

                // Create dist directory
                bat "if not exist %DIST_DIR% mkdir %DIST_DIR%"

                // Copy source files
                bat "xcopy /E /Y /I * %DIST_DIR%\\"

                // Minify CSS
                bat 'powershell -Command "(Get-Content style.css) -replace ''\\s+'', '' '' | Set-Content dist/style.min.css"'

                // Minify JS
                bat 'powershell -Command "(Get-Content script.js) -replace ''\\s+'', '' '' | Set-Content dist/script.min.js"'
            }
        }

        stage('Tests') {
            steps {
                echo "Test des fichiers..."

                bat "if not exist index.html (echo ERROR: index.html manquant & exit /b 1)"
                bat "if not exist dist/style.min.css (echo ERROR: CSS non minifie & exit /b 1)"
                bat "if not exist dist/script.min.js (echo ERROR: JS non minifie & exit /b 1)"

                echo "Tests OK."
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploiement..."

                bat "if not exist %DEPLOY_DIR% mkdir %DEPLOY_DIR%"
                bat "xcopy /E /Y /I %DIST_DIR% %DEPLOY_DIR%\\"

                echo "Deploiement termine dans : %DEPLOY_DIR%"
            }
        }
    }

    post {
        success {
            echo "Pipeline reussi."
        }
        failure {
            echo "Pipeline echoue."
        }
    }
}

