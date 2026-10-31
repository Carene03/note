# NOTE H873 — Kaggle depuis le runner public

## État confirmé le 31 octobre 2026

- GitHub runner public `Carene03/note` : run [41192153957](https://github.com/Carene03/note/actions/runs/41192153957) **success**.
- CLI Kaggle et `KAGGLE_API_TOKEN` : run [41192373175](https://github.com/Carene03/note/actions/runs/41192373175) **success**.
- Soumission du kernel Kaggle privé : run [41192555747](https://github.com/Carene03/note/actions/runs/41192555747) **success**.
- Calcul CPU privé effectivement **COMPLETE**, sortie JSON téléchargée et contrôlée : run [41192646827](https://github.com/Carene03/note/actions/runs/41192646827) **success**.
- Ces quatre contrôles sont techniques. **Aucun replay scientifique H873 ni score nouveau** n'a encore été produit.

## Préparer un seul vrai calcul H873

Workflow prêt : `.github/workflows/note-h873-kaggle-submit.yml`.

Le secret GitHub `KAGGLE_API_TOKEN` est déjà fourni par le propriétaire et son authentification est validée. Il faut en plus un **second secret `NOTE_SOURCE_READ_TOKEN`**, nécessaire parce que ce runner **public** n'a pas le droit de lire le dépôt **privé** `Andriamarosoa/note` avec son `GITHUB_TOKEN` natif.

Sur le compte GitHub `Andriamarosoa`, créer un **fine-grained personal access token** limité à **Only select repositories → Andriamarosoa/note**, avec **Contents: Read-only** et **Actions: Read-only**, expiration courte. Ne pas placer le jeton dans des logs, le chat ou un fichier commité.

Sur `Carene03/note` → **Settings → Secrets and variables → Actions → New repository secret**, le nom doit être exactement `NOTE_SOURCE_READ_TOKEN`. Coller sa valeur dans l'interface GitHub, jamais dans du code.

Quand ce secret existe, déclencher une seule soumission par la création d'un **nouveau** fichier `requests/h873/submit-YYYYMMDD-NN.txt` sur `main`. L'ajout du workflow seul **ne** déclenche **pas** le calcul.

## Fonctionnement et confidentialité

1. Le runner public clone `Andriamarosoa/note` **uniquement sur sa machine éphémère**, via le token de lecture.
2. Il utilise le code de passerelle fixé au commit `58c528c8334baa5c623bd5eafa4a9dc2d129385e` et l'expérience figée `05ef09cf8d750b3dc0220d38f37b56f6328e52f5`.
3. La passerelle authentifie les exports NOTE Deep Audit originaux et récupère les poids sources S840/S842, puis crée un **dataset Kaggle privé** (CLI `kaggle datasets create`, sans `--public`) et un **script Kaggle privé** (`kaggle kernels push`).
4. Le calcul sur Kaggle doit retrouver 56 pistes, 325278 trames, 3206 erreurs historiques K3→K4 et la parité SHA, pour le protocole strict **44,1 kHz / PCM256 / zéro futur / zéro hold / FIT80 fermé**.
5. Ce premier workflow sert à **soumettre** : l'exécution sur Kaggle et le contrôle des résultats se font ensuite séparément. **Ne pas publier les résultats note par note dans ce dépôt public.** Les fichiers sensibles resteront sur Kaggle privé ou seront rapportés dans le dépôt NOTE privé via une procédure à autoriser séparément.

**Ne jamais** changer les modèles officiels ni annoncer de nouveau score musical sur simple soumission.
