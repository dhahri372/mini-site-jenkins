pipeline {
agent any


environment {
    DIST_DIR   = "dist"
    DEPLOY_DIR = "C:\\mini-site-deploy"
}

stages {

    stage('Init Workspace') {
        steps {
            echo "Nettoyage du workspace..."
            deleteDir()
        }
    }

    stage('Checkout') {
        steps {
            echo "Récupération du code depuis GitHub..."
            git branch: 'main', url: 'https://github.com/dhahri372/mini-site-jenkins.git'
        }
    }

    stage('Build') {
        steps {
            echo "🏗️ Build du projet + minification CSS/JS..."

            bat "if not exist %DIST_DIR% mkdir %DIST_DIR%"
            bat "xcopy /E /Y /I * %DIST_DIR%\\"

            bat """powershell -Command "(Get-Content 'style.css') -replace '\\s+', ' ' | Set-Content 'dist/style.min.css'" """
            bat """powershell -Command "(Get-Content 'script.js') -replace '\\s+', ' ' | Set-Content 'dist/script.min.js'" """
        }
    }

    stage('Tests') {
        steps {
            echo "Exécution des tests..."

            bat "if not exist index.html (echo ERREUR: index.html manquant & exit /b 1)"
            bat "if not exist dist/style.min.css (echo ERREUR: CSS non minifié & exit /b 1)"
            bat "if not exist dist/script.min.js (echo ERREUR: JS non minifié & exit /b 1)"

            echo "Tous les tests sont passés."
        }
    }

    stage('Archive Build') {
        steps {
            echo "Archivage des fichiers de build..."
            archiveArtifacts artifacts: 'dist/**', fingerprint: true
        }
    }

    stage('Deploy') {
        steps {
            echo "Déploiement vers le dossier local..."

            bat "if not exist %DEPLOY_DIR% mkdir %DEPLOY_DIR%"
            bat "xcopy /E /Y /I %DIST_DIR% %DEPLOY_DIR%\\"

            echo "Site déployé dans : %DEPLOY_DIR%"
        }
    }
}

post {
    success {
        echo "Pipeline terminé avec succès !"
    }
    failure {
        echo "Le pipeline a échoué."
    }
}


}



