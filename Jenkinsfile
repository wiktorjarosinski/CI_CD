pipeline {
    agent any

    environment {
        APP_VERSION  = '1.0.0'
        DEPLOY_ENV   = 'staging'
        REPO_URL     = 'https://github.com/wiktorjarosinski/CI_CD.git'
        NOTIFY_EMAIL = 'xwiciux13@studia.com'
    }

    stages {

        stage('Checkout') {
            steps {
                script {
                    try {
                        echo ">>> [CHECKOUT] Pobieranie kodu z: ${env.REPO_URL}"
                        echo ">>> Wersja: ${env.APP_VERSION} | Środowisko: ${env.DEPLOY_ENV}"
                        git branch: 'main',
                            url: "${env.REPO_URL}"
                        echo ">>> Autor ostatniego commita: ${sh(script: 'git log -1 --pretty=format:"%an"', returnStdout: true).trim()}"
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("❌ Checkout nieudany: ${e.message}")
                    }
                }
            }
        }

        stage('Build') {
            when {
                expression { currentBuild.result == null }
            }
            steps {
                script {
                    try {
                        echo ">>> [BUILD] Kompilacja aplikacji v${env.APP_VERSION}..."
                        sh 'echo "Symulacja kompilacji..." && sleep 2 && echo "✅ Build zakończony sukcesem"'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("❌ Build nieudany: ${e.message}")
                    }
                }
            }
        }

        stage('Test') {
            when {
                expression { currentBuild.result == null }
            }
            steps {
                script {
                    try {
                        echo ">>> [TEST] Uruchamianie testów automatycznych..."
                        sh 'echo "Symulacja testów..." && sleep 2 && echo "✅ Wszystkie testy przeszły"'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("❌ Testy nieudane: ${e.message}")
                    }
                }
            }
        }

        stage('Deploy') {
            when {
                expression { currentBuild.result == null }
            }
            steps {
                script {
                    try {
                        echo ">>> [DEPLOY] Wdrażanie v${env.APP_VERSION} na środowisko: ${env.DEPLOY_ENV}..."
                        sh 'echo "Symulacja wdrożenia..." && sleep 1 && echo "✅ Deploy zakończony sukcesem"'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        error("❌ Deploy nieudany: ${e.message}")
                    }
                }
            }
        }
    }

    post {
        success {
            echo """
============================================
✅ PIPELINE ZAKOŃCZONY SUKCESEM
============================================
Projekt    : ${env.JOB_NAME}
Build      : #${env.BUILD_NUMBER}
Wersja     : ${env.APP_VERSION}
Środowisko : ${env.DEPLOY_ENV}
Czas       : ${currentBuild.durationString}
URL        : ${env.BUILD_URL}
============================================
            """
            // mail to: "${env.NOTIFY_EMAIL}",
            //      subject: "[Jenkins] ✅ ${env.JOB_NAME} #${env.BUILD_NUMBER} – SUCCESS",
            //      body: """
            //          Pipeline zakończony sukcesem.
            //          Projekt: ${env.JOB_NAME}
            //          Build: #${env.BUILD_NUMBER}
            //          Wersja: ${env.APP_VERSION}
            //          Środowisko: ${env.DEPLOY_ENV}
            //          Czas wykonania: ${currentBuild.durationString}
            //          Logi: ${env.BUILD_URL}
            //      """
        }
        failure {
            echo """
============================================
❌ PIPELINE ZAKOŃCZONY BŁĘDEM
============================================
Projekt    : ${env.JOB_NAME}
Build      : #${env.BUILD_NUMBER}
Wersja     : ${env.APP_VERSION}
Czas       : ${currentBuild.durationString}
URL        : ${env.BUILD_URL}
============================================
            """
            // mail to: "${env.NOTIFY_EMAIL}",
            //      subject: "[Jenkins] ❌ ${env.JOB_NAME} #${env.BUILD_NUMBER} – FAILURE",
            //      body: """
            //          Pipeline zakończony błędem!
            //          Projekt: ${env.JOB_NAME}
            //          Build: #${env.BUILD_NUMBER}
            //          Sprawdź logi: ${env.BUILD_URL}
            //      """
        }
        always {
            echo ">>> [POST] Status końcowy: ${currentBuild.currentResult} | Czas: ${currentBuild.durationString}"
        }
    }
}
