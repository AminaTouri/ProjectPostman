pipeline {
    agent any

    triggers {
        cron('20 13 * * *')  //
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

                    // Exécution de Newman
                    bat '"C:\\Users\\EXPERT~1\\AppData\\Roaming\\npm\\newman.cmd" run "%WORKSPACE%\\Exo1.postman_collection.json" -e "%WORKSPACE%\\PP.postman_environment.json" -r htmlextra --reporter-htmlextra-export "%WORKSPACE%\\newman-report.html"'
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

            // Envoi du mail avec le rapport en pièce jointe via Email Extension
            emailext(
                to: 'touriamina123@gmail.com',
                subject: "Rapport Build ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """Bonjour,

Le build ${env.JOB_NAME} #${env.BUILD_NUMBER} est terminé.

Vous pouvez consulter le rapport Newman en pièce jointe ou via Jenkins : ${env.BUILD_URL}

Cordialement.""",
                attachmentsPattern: 'newman-report.html'
            )
        }

        failure {
            echo 'Certaines requêtes Newman ont échoué. Vérifie le rapport HTML pour les détails.'
        }
    }
}
