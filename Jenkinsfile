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
            echo "Build du projet + minification CSS/JS..."

            // Crée le dossier dist s'il n'existe pas
            bat "if not exist %DIST_DIR% mkdir %DIST_DIR%"

            // Copier uniquement les fichiers nécessaires
            bat "copy index.html %DIST_DIR%\\"
            bat "copy style.css %DIST_DIR%\\"
            bat "copy script.js %DIST_DIR%\\"

            // Minification CSS
            bat """powershell -Command "(Get-Content '%DIST_DIR%\\style.css') -replace '\\s+', ' ' | Set-Content '%DIST_DIR%\\style.min.css'" """

            // Minification JS
            bat """powershell -Command "(Get-Content '%DIST_DIR%\\script.js') -replace '\\s+', ' ' | Set-Content '%DIST_DIR%\\script.min.js'" """
        }
    }

    stage('Tests') {
        steps {
            echo "Exécution des tests..."


           // Vérifie que les fichiers existent
           bat "if not exist %DIST_DIR%\\index.html (echo ERREUR: index.html manquant & exit /b 1)"
           bat "if not exist %DIST_DIR%\\style.min.css (echo ERREUR: CSS non minifié & exit /b 1)"
           bat "if not exist %DIST_DIR%\\script.min.js (echo ERREUR: JS non minifié & exit /b 1)"

           bat '''powershell -Command "if (-not (Select-String -Path 'dist\\index.html' -Pattern '<html>' -Quiet -SimpleMatch -Encoding UTF8)) { Write-Error 'HTML manquant <html>'; exit 1 }"'''


           // Vérifie que le CSS minifié n'est pas vide
           bat '''powershell -Command "if ((Get-Content '%DIST_DIR%\\style.min.css').Length -eq 0) { Write-Error 'CSS minifié vide'; exit 1 }"'''

           // Vérifie que le JS minifié n'est pas vide
           bat '''powershell -Command "if ((Get-Content '%DIST_DIR%\\script.min.js').Length -eq 0) { Write-Error 'JS minifié vide'; exit 1 }"'''

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

            // Copier tout le dist dans le dossier de déploiement
            bat "xcopy /E /Y /I %DIST_DIR% %DEPLOY_DIR%\\"

            echo "Site déployé dans : %DEPLOY_DIR%"
        }
    }
}

post {
    success {
        echo "Pipeline terminé avec succès ! 🎉"
    }
    failure {
        echo "Le pipeline a échoué."
    }
}


}


  
