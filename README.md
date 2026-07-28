# TP - GitHub Actions

Pipeline CI qui build une image Docker, lance les tests, démarre le conteneur et vérifie que l'app répond.

## Le pipeline

Déclenché sur push et pull request vers `main`, plus lancement manuel (`workflow_dispatch`).
Runner `ubuntu-24.04`, timeout 10 min.

Aucune. Le pipeline est passé au vert dès le premier run, je n'ai pas eu d'erreur à corriger.

Les points que j'avais vérifiés avant de push :

- **Ports** : le conteneur écoute sur 5000, la publication est en `8080:5000`. Le curl de l'étape d'attente tape donc sur `127.0.0.1:8080`, pas 5000.
- **pytest dans l'image** : `pytest` est bien dans `requirements.txt`, sinon l'étape de tests plante avec un `executable file not found`.
- **Copie des tests** : `test_app.py` est copié dans l'image (`COPY app.py test_app.py ./`), sinon pytest ne collecte aucun test.
- **Bind 0.0.0.0** : Flask écoute sur `0.0.0.0` et pas `127.0.0.1`, sinon le port publié ne renvoie rien.
- **User non-root** : `useradd` est fait avant le `USER appuser`, et l'app n'écrit rien sur le disque, donc pas de problème de permissions.
- **Nettoyage** : l'étape de nettoyage est en `always()` pour que le conteneur soit supprimé même si une étape précédente échoue.
