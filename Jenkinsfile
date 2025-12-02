pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Récupération du code depuis GitHub...'
                git branch: 'main', url: 'https://github.com/dhahri372/mini-site-jenkins.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Stage Build: ici on pourrait minifier CSS/JS si nécessaire...'
            }
        }

        stage('Test') {
            steps {
                echo 'Stage Test: Vérification basique des fichiers...'
                bat 'dir'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Stage Deploy: Copie du site vers un dossier local (exemple)...'
                bat 'if not exist C:\\mini-site-deploy mkdir C:\\mini-site-deploy'
                bat 'xcopy /E /Y /I * C:\\mini-site-deploy\\'
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminé ! 🎉'
        }
    }
}


