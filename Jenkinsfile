pipeline {
    agent any

    parameters {
        string(name: 'APP_VERSION', defaultValue: '1.0.0', description: 'Version à construire')
        choice(name: 'ENVIRONNEMENT', choices: ['dev', 'recette', 'prod'], description: 'Environnement cible')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Exécuter les tests ?')
    }

    stages {
        stage('Informations') {
            steps {
                echo "Version : ${params.APP_VERSION}"
                echo "Environnement : ${params.ENVIRONNEMENT}"
                echo "Tests activés : ${params.RUN_TESTS}"
            }
        }

        stage('Build') {
            steps {
                sh '''
                    mkdir -p build
                    echo "app ${APP_VERSION}" > build/app.txt
                '''
            }
        }

        stage('Tests') {
            when {
                expression { params.RUN_TESTS }
            }
            steps {
                echo 'Exécution des tests...'
                sh 'test -f build/app.txt'
            }
        }

        stage('Déploiement recette') {
            when {
                allOf {
                    expression { params.ENVIRONNEMENT == 'recette' }
                    branch 'main'
                }
            }
            steps {
                echo 'Déploiement en recette...'
            }
        }

        stage('Déploiement prod') {
            when {
                expression { params.ENVIRONNEMENT == 'prod' }
            }
            steps {
                echo 'Déploiement en production...'
            }
        }
    }
}