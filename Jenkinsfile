pipeline {
    agent any  // Utilise n'importe quel agent disponible pour exécuter le pipeline

    // Déclencheurs automatiques
    triggers {
        cron('20 13 * * *')  // Exécution automatique tous les jours à 13:20 (heure du serveur Jenkins)
    }

    // Définition des outils nécessaires
    tools {
        nodejs 'Node24'  // Utiliser la version NodeJS configurée dans Jenkins nommée "Node24"
    }

    // Paramètres de build
    parameters {
        choice(
            name: 'ENV',  // Nom du paramètre
            choices: ['PP', 'DEV', 'TEST', 'PROD'],  // Choix possibles pour l'environnement
            description: 'Choisissez l\'environnement pour exécuter la collection Newman'  // Description affichée à l'utilisateur
        )
    }

    // Définition des étapes du pipeline
    stages {
        stage('Checkout') {  // Étape pour récupérer le code source depuis GitHub
            steps {
                git branch: 'main', url: 'https://github.com/AminaTouri/ProjectPostman.git'  
                // Clone la branche 'main' du dépôt GitHub spécifié dans le workspace Jenkins
            }
        }

        stage('Run Postman Collection') {  // Étape pour exécuter la collection Postman avec Newman
            steps {
                script {  // Permet d'utiliser du code Groovy pour plus de flexibilité
                    echo "Environnement choisi: ${params.ENV}"  
                    // Affiche l'environnement choisi dans la console Jenkins pour debug

                    // Exécute la collection Postman avec Newman
                    // - Le fichier de collection est "Exo1.postman_collection.json"
                    // - Le fichier d'environnement est choisi dynamiquement selon le paramètre ENV
                    // - Le rapport est généré en format HTML via le reporter "htmlextra"
                    // - Le rapport est exporté dans le workspace Jenkins sous "newman-report.html"
                    bat """
                    npx newman run "%WORKSPACE%\\Exo1.postman_collection.json" -e "%WORKSPACE%\\${params.ENV}.postman_environment.json" -r cli,htmlextra --reporter-htmlextra-export "%WORKSPACE%\\newman-report.html"
                    """
                }
            }
        }
    }

    // Actions post-build
    post {
        always {  // Ces actions se feront toujours, qu'il y ait succès ou échec du build
            // Publication du rapport HTML dans Jenkins
            publishHTML(target: [
                allowMissing: false,  // Échoue si le rapport n'est pas trouvé
                alwaysLinkToLastBuild: true,  // Crée un lien vers le dernier build
                keepAll: true,  // Conserve tous les rapports de builds précédents
                reportDir: '.',  // Répertoire contenant le rapport (ici le workspace)
                reportFiles: 'newman-report.html',  // Nom du fichier de rapport généré
                reportName: 'Newman HTML Report'  // Nom affiché dans Jenkins
            ])

            // Envoi d'un mail avec le rapport en pièce jointe
            emailext(
                to: 'touriamina123@gmail.com',  // Destinataire du mail
                subject: "Rapport Build ${env.JOB_NAME} #${env.BUILD_NUMBER}",  // Sujet du mail avec nom du job et numéro de build
                body: """Bonjour,

Le build ${env.JOB_NAME} #${env.BUILD_NUMBER} est terminé.

Vous pouvez consulter le rapport Newman en pièce jointe ou via Jenkins : ${env.BUILD_URL}

Cordialement.""",  // Corps du mail
                attachmentsPattern: 'newman-report.html'  // Fichier attaché au mail
            )
        }

        failure {  // Actions spécifiques en cas d'échec du build
            echo 'Certaines requêtes Newman ont échoué. Vérifie le rapport HTML pour les détails.'
            // Message affiché dans la console pour signaler un problème dans la collection Newman
        }
    }
}
