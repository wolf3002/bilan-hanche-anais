# Bilan hanche — Anaïs

Formulaire d'auto-évaluation clinique pour le diagnostic différentiel d'une douleur de hanche latérale.

## Utilisation

1. Anaïs ouvre le lien GitHub Pages sur son téléphone
2. Elle remplit les 20 questions + les 8 tests cliniques (avec Valentin pour la partie tests)
3. Elle clique sur **Envoyer à Valentin**
4. Le bilan arrive en notification ntfy.sh sur le téléphone de Valentin

## Réception côté Valentin

- Web : ouvrir `https://ntfy.sh/<topic>` dans le navigateur (l'onglet doit rester ouvert pour les notifs temps réel)
- App : installer **ntfy** (Android / iOS), s'abonner au topic
- Le markdown du bilan est joint au message sous forme de fichier téléchargeable

## Sécurité

Le topic ntfy est public (n'importe qui avec l'URL peut lire/écrire). Pour faire tourner les clés : changer la constante `NTFY_TOPIC` dans `bilan-hanche.html` et re-déployer.
