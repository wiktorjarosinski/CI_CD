// ============================================================
// Jenkinsfile – Zaawansowany pipeline CI/CD
// Projekt: CI_CD | Autor: Wiktor Jarosiński
// Opis: Pipeline z obsługą błędów, powiadomieniami e-mail
//       i integracją z repozytorium GitHub
// ============================================================

pipeline {
    agent any  // Uruchom na dowolnym dostępnym agencie Jenkins

    // -- Zmienne środowiskowe dostępne w całym pipeline --
    environment {
        APP_VERSION  = '1.0.0'                                        // Wersja aplikacji
        DEPLOY_ENV   = 'staging'                                      // Środowisko docelowe
        REPO_URL     = 'https://github.com/wiktorjarosinski/CI_CD.git' // URL repozytorium
        NOTIFY_EMAIL = 'xwiciux13@studia.com'                         // Adres powiadomień
    }

    stages {

        // -------------------------------------------------------
        // ETAP 1: Checkout
        // Pobiera kod źródłowy z repozytorium GitHub (branch main)
        // Wyświetla autora ostatniego commita z git log
        // -------------------------------------------------------
        stage('Checkout') {
            steps {
                script {
                    try {
                        echo ">>> [CHECKOUT] Pobieranie kodu z: ${env.REPO_URL}"
                        echo ">>> Wersja: ${env.APP_VERSION} | Środowisko: ${env.DEPLOY_ENV}"

                        // Pobieranie kodu z brancha main
                        git branch: 'main',
                            url: "${env.REPO_URL}"

                        // Odczytanie autora ostatniego commita przez git log
                        echo ">>> Autor ostatniego commita: ${sh(script: 'git log -1 --pretty=format:"%an"', returnStdout: true).trim()}"

                    } catch (Exception e) {
                        // Ustawienie statusu FAILURE i przerwanie pipeline'u
                        currentBuild.result = 'FAILURE'
                        error("❌ Checkout nieudany: ${e.message}")
                    }
                }
            }
        }

        // -------------------------------------------------------
        // ETAP 2: Build
        // Symulacja kompilacji aplikacji
        // Uruchamia się tylko jeśli Checkout zakończył się sukcesem
        // (currentBuild.result == null oznacza brak błędów)
        // -------------------------------------------------------
        stage('Build') {
            when {
                // Warunkowe wykonanie – pomiń jeśli poprzedni etap się posypał
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

        // -------------------------------------------------------
        // ETAP 3: Test
        // Symulacja uruchamiania testów automatycznych
        // Uruchamia się tylko jeśli Build zakończył się sukcesem
        // -------------------------------------------------------
        stage('Test') {
            when {
                // Warunkowe wykonanie – pomiń jeśli poprzedni etap się posypał
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

        // -------------------------------------------------------
        // ETAP 4: Deploy
        // Symulacja wdrożenia aplikacji na środowisko staging
        // Uruchamia się tylko jeśli wszystkie poprzednie etapy OK
        // -------------------------------------------------------
        stage('Deploy') {
            when {
                // Warunkowe wykonanie – pomiń jeśli poprzedni etap się posypał
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

    // -------------------------------------------------------
    // POST – wykonuje się zawsze po zakończeniu pipeline'u
    // success : wszystkie etapy zakończone sukcesem
    // failure : co najmniej jeden etap zakończony błędem
    // always  : niezależnie od wyniku
    // -------------------------------------------------------
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
            // Powiadomienie e-mail po pomyślnym wdrożeniu
            // Odkomentuj po skonfigurowaniu SMTP w Jenkins
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
            // Powiadomienie e-mail w przypadku niepowodzenia
            // Odkomentuj po skonfigurowaniu SMTP w Jenkins
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
            // Wyświetl końcowy status niezależnie od wyniku
            echo ">>> [POST] Status końcowy: ${currentBuild.currentResult} | Czas: ${currentBuild.durationString}"
        }
    }
}
