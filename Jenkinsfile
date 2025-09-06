pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AminaTouri/ProjectPostman.git'
            }
        }

        stage('Run Postman Collection') {
            steps {
                // Chemin court pour éviter problème d'espace
                bat '"C:\\Users\\EXPERT~1\\AppData\\Roaming\\npm\\newman.cmd" run "%WORKSPACE%\\Exo1.postman_collection.json" -e "%WORKSPACE%\\PP.postman_environment.json"'
            }
        }
    }
}
