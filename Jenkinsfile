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
                bat 'newman run Exo1.postman_collection.json -e PP.postman_environment(1).json'
            }
        }
    }
}
