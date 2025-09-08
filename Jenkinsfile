pipeline {
    agent any
    environment {
        /*
            Permet de créer des variables d'environnements qu'on peut utiliser dans tous les stages
        */
        // Lien API vers la collection Postman
        COLLECTION_API_LINK = 'https://api.postman.com/collections/48076698-5d1bcd07-9ed1-4c40-8ab4-0e691093ad7b?access_key=PMAT-01K4HYW4TCYG12KAZP8Z0VKFNB'
    }
    stages {
        stage("Get github repository") {
            /*
                Permet de récupérer le repo github
            */
            steps {
                git branch: 'main', url: 'https://github.com/Ala-Chahbani/Jenkins-test-with-postman'
            }
        }

        stage("Install/Update dependencies") {
            /*
                Permet d'installer/màj les dépendences nécessaires : newman + newman-reporter-htmlextra
            */
            steps {
                bat 'npm install -g newman'
                bat 'npm install -g newman-reporter-htmlextra'
             }
        }

        stage("Vérification de l'environnement de test utilisé") {
            steps {
                bat 'echo %TEST_ENV%'
            }
        }
        
        stage("Run API tests") {
            steps {
                /*
                    Exécution des tests API via newman et génération des rapports html dans un dossier "newman"
                */
                bat 'npx newman run %COLLECTION_API_LINK% -d .\\data\\users_data.json --reporters=htmlextra'
             }
             
             post {
                 /*
                    post s'exécute toujours après les étapes de steps selon certaines condition
                 */
                 // always permet l'exécution du contenu de post peu-importe le résultat du contenu de steps
                always {
                    // utilise le plugin HTML Publisher pour archiver les rapports html générés par steps dans une page dans le dashboard jenkins
                    publishHTML (target: [
                        allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'newman',
                        reportFiles: '*.html',
                        reportName: 'Newman Report'
                    ])
                 }
             }
        }
    }
}