# CI_CD Pipeline

Zaawansowany pipeline Jenkins z obsługą błędów, systemem powiadomień e-mail
i integracją z repozytorium GitHub.

## Struktura pipeline'u

Pipeline składa się z 4 etapów wykonywanych sekwencyjnie:

| Etap | Opis |
|------|------|
| Checkout | Pobiera kod z brancha `main` na GitHubie, wyświetla autora ostatniego commita |
| Build | Symulacja kompilacji aplikacji (sleep 2s) |
| Test | Symulacja uruchamiania testów automatycznych (sleep 2s) |
| Deploy | Symulacja wdrożenia na środowisko `staging` |

## Obsługa błędów

- Każdy etap opakowany jest w blok `try-catch`
- W przypadku błędu ustawiany jest status `FAILURE` i pipeline jest przerywany
- Kolejne etapy wykonują się tylko jeśli poprzednie zakończyły się sukcesem (`when { expression { currentBuild.result == null } }`)

## System powiadomień

Bloki `post` obsługują trzy scenariusze:

- `success` – wyświetla podsumowanie z wersją, środowiskiem i czasem wykonania
- `failure` – wyświetla informację o błędzie z linkiem do logów
- `always` – wyświetla końcowy status niezależnie od wyniku

Powiadomienia e-mail są zaimplementowane i zakomentowane – wymagają konfiguracji SMTP w Jenkins (`Zarządzaj Jenkinsem → System → E-mail`).

## Zmienne środowiskowe

| Zmienna | Wartość | Opis |
|---------|---------|------|
| `APP_VERSION` | `1.0.0` | Wersja aplikacji |
| `DEPLOY_ENV` | `staging` | Środowisko docelowe |
| `REPO_URL` | `https://github.com/wiktorjarosinski/CI_CD.git` | URL repozytorium |
| `NOTIFY_EMAIL` | `xwiciux13@studia.com` | Adres powiadomień |

## Uruchomienie

1. Sklonuj repozytorium
2. W Jenkins utwórz projekt typu **Pipeline**
3. Ustaw **Pipeline script from SCM** → Git → podaj URL repo
4. Branch: `*/main`, Script Path: `Jenkinsfile`
5. Kliknij **Uruchom teraz**
