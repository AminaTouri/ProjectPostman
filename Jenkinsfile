pipeline {
    agent any
    triggers {
        cron('0 13 * * *')  // Tous les jours à 13h (heure du serveur Jenkins)
    }

    parameters {
        choice(
            name: 'ENV',
            choices: ['PP', 'DEV', 'TEST', 'PROD'],
            description: 'Choisissez l\'environnement pour exécuter la collection Newman'
        )
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AminaTouri/ProjectPostman.git'
            }
        }


        stage('Run Postman Collection') {
            steps {
                script {
                    // Construire l'URL selon l'environnement choisi
                    def baseUrl = "https://${params.ENV}.mockapi.io/testapi/api/v1/clients"

                    // Afficher l'environnement et l'URL pour debug
                    echo "Environnement choisi: ${params.ENV}"
                    echo "URL utilisée pour Newman: ${baseUrl}"

                    // Exécution de Newman avec environnement inline
                     bat '"C:\\Users\\EXPERT~1\\AppData\\Roaming\\npm\\newman.cmd" run "%WORKSPACE%\\Exo1.postman_collection.json" -e "%WORKSPACE%\\PP.postman_environment.json"'
                }
            }
        }
    }

    post {
        always {
            // Publication du rapport HTML
            publishHTML(target: [
                allowMissing: false,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: '.',
                reportFiles: 'newman-report.html',
                reportName: 'Newman HTML Report'
            ])
        }
        failure {
            // Message clair si certaines requêtes échouent
            echo 'Certaines requêtes Newman ont échoué. Vérifie le rapport HTML pour les détails.'
        }
    }
}
