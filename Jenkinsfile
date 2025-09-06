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
                // Utilise %WORKSPACE% pour pointer vers les fichiers dans le workspace
                bat '"C:\\Users\\expert info\\AppData\\Roaming\\npm\\newman.cmd" run "%WORKSPACE%\\Exo1.postman_collection.json" -e "%WORKSPACE%\\PP.postman_environment.json"'
            }
        }
    }
}
