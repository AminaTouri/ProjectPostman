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
			bat '"C:\\Users\\expert info\\AppData\\Roaming\\npm\\newman.cmd" run "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Exo1_Postman_Pipeline\\Exo1.postman_collection.json" -e "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\Exo1_Postman_Pipeline\\PP.postman_environment.json"'
            }
        }
    }
}
