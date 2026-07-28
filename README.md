# TP - GitHub Actions

Pipeline CI qui build une image Docker, lance les tests, démarre le conteneur et vérifie que l'app répond.

## Le pipeline

Déclenché sur push et pull request vers `main`, plus lancement manuel (`workflow_dispatch`).
Runner `ubuntu-24.04`, timeout 10 min.

Le pipeline est passé au vert dès le premier run, je n'ai pas eu d'erreur à corriger.
