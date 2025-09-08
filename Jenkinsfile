pipeline {
    agent any

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
                git branch: 'main', url: 'https://github.com/AminaTouri/ProjectPostman2.git'
            }
        }

        stage('Install Newman & Reporter') {
            steps {
                bat 'npm install -g newman newman-reporter-htmlextra'
            }
        }

        stage('Run Postman Collection') {
            steps {
                script {
                    def baseUrl = "https://${params.ENV}.mockapi.io/testapi/api/v1/clients"
                    // Exécution Newman avec URL dynamique injectée dans l'environnement inline
                    bat """
                    npx newman run "Exo1.postman_collection.json" ^
                    -e "{ \\"values\\": [ { \\"key\\": \\"PP\\", \\"value\\": \\"${baseUrl}\\", \\"enabled\\": true } ] }" ^
                    -r htmlextra ^
                    --reporter-htmlextra-export "newman-report.html" || exit 0
                    """
                }
            }
        }
    }

    post {
        always {
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
            echo 'Certaines requêtes Newman ont échoué. Vérifie le rapport HTML pour les détails.'
        }
    }
}
