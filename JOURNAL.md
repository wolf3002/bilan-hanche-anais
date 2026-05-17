# JOURNAL — Projet Bilan Hanche Anaïs

> Suivi de l'avancée du projet, des décisions cliniques et techniques, et des découvertes.
> Format : entrées datées du plus récent au plus ancien.

---

## 📋 Contexte du projet

**Sujet** : Anaïs (1m65, 65 kg, ASVP en alternance) souffre de douleurs latérales de hanche, déclenchées par les longues marches (10 000+ pas en une fois) et la station debout prolongée. Le côté gauche apparaît en premier puis ça se bilatéralise. Semelles partiellement efficaces. Sédentaire la journée (voiture/bureau) avec pics ponctuels d'activité.

**Objectif** : Aider Anaïs à comprendre l'origine de sa douleur, préparer son rendez-vous kiné avec un bilan structuré, et lui fournir un protocole de rééducation evidence-based.

**Acteurs** :
- **Anaïs** : utilisatrice finale du formulaire, patiente
- **Valentin** : porte le projet, fait l'intermédiaire avec Claude
- **Claude** : aide au diagnostic différentiel, conçoit l'outil de collecte, et raffinera le protocole de rééducation après réception du bilan

---

## 🎯 État actuel (2026-05-17)

### Diagnostic préliminaire
**Hypothèse principale** : Syndrome douloureux du grand trochanter (GTPS) avec composante de déconditionnement posturale liée à la sédentarité.

**Différentiel à éliminer** :
- Conflit fémoro-acétabulaire (FAI) cam/pincer
- Lésion du labrum acétabulaire
- Dysfonction sacro-iliaque
- Référé lombaire L1-L3
- Syndrome de la bandelette ilio-tibiale (ITBS)
- Syndrome du piriforme
- Dysplasie acétabulaire infra-clinique
- Fracture de stress du col fémoral (red flag, surtout si aménorrhée + sport intense)

### Outil de collecte déployé
- **URL formulaire** : https://wolf3002.github.io/bilan-hanche-anais/
- **Repo GitHub** : https://github.com/wolf3002/bilan-hanche-anais
- **Canal de réception** : ntfy.sh, topic `bilan-anais-h4n7Kp9xR3vM`
- **URL réception (Valentin)** : https://ntfy.sh/bilan-anais-h4n7Kp9xR3vM

### Statut du formulaire
- ✅ Sections 1-3 actives (caractérisation douleur, mécanique, contexte) — 20 questions
- ⏸️ Section 4 (tests cliniques) désactivée — Anaïs ne peut pas les faire actuellement, réactivable via `ENABLE_TESTS_SECTION = true` dans `index.html`
- ✅ Persistance localStorage : sauvegarde auto + restauration
- ✅ Champs "Précisions / autre" par section
- ✅ Envoi automatique via ntfy.sh, bilan reçu en .md attaché
- ✅ Drapeaux automatiques (nocturne, aménorrhée, perte de poids, blocages, dérobements)

### TODO à court terme
- [ ] Valentin envoie le lien à Anaïs
- [ ] Anaïs remplit les sections 1-3
- [ ] Valentin reçoit le bilan via ntfy → me le transmet
- [ ] Je raffine le diagnostic différentiel à partir des réponses
- [ ] Je propose le protocole de rééducation initial (probablement GTPS evidence-based : isométriques → isotoniques → heavy slow resistance, ref. Mellor 2018 LEAP trial)

### TODO moyen terme (si pertinent)
- [ ] Réactiver section tests cliniques quand Anaïs pourra les faire (seule ou avec aide)
- [ ] Bilan complémentaire après examen kiné
- [ ] Suivi à 6 semaines avec re-passage du formulaire pour mesurer l'évolution

---

## 📚 Historique des sessions

### 2026-05-17 — Session 4 : Désactivation section tests
**Fait** :
- Section 4 (tests cliniques) masquée via flag JS `ENABLE_TESTS_SECTION = false`
- Intro reformulée : 5-10 min, 20 questions, plus de mention de tests
- Barre de progression filtrée pour ignorer la section désactivée
- Générateur : section 4 indique "Non réalisés pour le moment" si désactivée
- Drapeau test du saut conditionnel sur ENABLE_TESTS_SECTION

**Décisions** :
- **Approche réversible** : la section reste dans le code (instructions, champs, logique conservés), juste cachée. Pour réactiver : changer 1 ligne + push.
- Raison : Anaïs pourra peut-être faire les tests plus tard (avec un kiné, ou en se filmant)

### 2026-05-17 — Session 3 : Persistance localStorage
**Fait** :
- Sauvegarde automatique de tous les champs (radio, checkbox, text, textarea) dans localStorage à chaque saisie (debounce 400 ms)
- Restauration au chargement initial
- Bouton "Effacer toutes mes réponses" avec confirmation
- Toast visuel "💾 Sauvegardé" à chaque save
- Sauvegarde de sécurité avant fermeture (`beforeunload`)

**Découvertes** :
- DOMContentLoaded ne se déclenche pas si le script est en fin de body après le DOM parsing → appel direct des fonctions d'init plus fiable
- Mode navigation privée efface les données : à signaler à Anaïs si pertinent

### 2026-05-17 — Session 2 : Mode solo + précisions
**Fait** :
- Adaptation des 8 tests cliniques pour version "Anaïs seule" (miroir, téléphone qui filme, auto-mobilisation)
- Ober remplacé par Ober modifié (passive, sans aide)
- Champ "Précisions / autre" ajouté en bas de chaque section
- Intro reformulée : Partie 2 faisable seule ou avec n'importe qui

**Découvertes** :
- L'utilisateur ne sera pas forcément accompagné par Valentin
- Tests qui nécessitent vraiment 2 personnes : FADIR (compromis acceptable en auto-mobilisation), Ober (compromis avec version modifiée)

### 2026-05-17 — Session 1 : Mise en place de l'outil
**Fait** :
- Création du formulaire HTML autoportée mobile-first (`index.html`, 44 Ko → ~55 Ko après ajouts)
- 20 questions structurées + 8 tests cliniques avec instructions intégrées
- Générateur de récap markdown avec tableau des tests + drapeaux automatiques
- Configuration ntfy.sh comme canal de réception (zéro setup côté Anaïs)
- Repo GitHub `bilan-hanche-anais` + GitHub Pages activé
- Mise au point sur structure git (NUL bytes dans COMMIT_EDITMSG sur le NAS, résolus en supprimant le fichier)

**Décisions techniques** :
- **Format** : page HTML autoportée plutôt que Google Forms — contrôle total, aucun compte à créer côté utilisateur, marche offline, persistance possible
- **Canal de réception** : ntfy.sh préféré à Telegram (pas de bot, pas de token visible publiquement) et préféré à Email Web3Forms (pas de compte à créer). Le topic obscur tient lieu de "clé".
- **Sécurité** : le topic ntfy.sh est public mais obscur ; révocation = renommer la constante NTFY_TOPIC + redéploiement

### 2026-05-17 — Session 0 : Diagnostic différentiel approfondi
**Fait** :
- Analyse critique du diagnostic GTPS posé par une conversation antérieure : plausible mais prématuré (pas de différentiel élargi, pas d'examen clinique, pas de red flags)
- Construction d'un différentiel à 10 hypothèses
- Définition d'un protocole d'auto-examen en 8 tests (palpation trochanter, Trendelenburg, single-leg squat, FADIR, FABER, Ober, piriforme, saut)
- Identification des red flags : douleur nocturne, aménorrhée, perte de poids, blocages, fracture de stress

**Décisions cliniques** :
- Approche : différentiel large d'abord, puis raffinement par auto-examen, puis kiné pour examen physique direct
- Important : ne PAS sauter directement à un diagnostic et un protocole — d'abord caractériser

---

## 🔑 Décisions de référence

### Cliniques
- **Diagnostic principal présumé** : GTPS / tendinopathie moyen fessier avec composante déconditionnement
- **Protocole de rééducation envisagé** : Mellor 2018 LEAP trial — isométriques (S1-2) → isotoniques (S3-6) → heavy slow resistance (S6+)
- **Anti-pattern à éviter** : étirements agressifs en adduction (compriment le tendon contre le trochanter), clamshells trop limités en activation EMG

### Techniques
- **Stack** : HTML/CSS/JS vanilla, zéro framework, autoportée
- **Hébergement** : GitHub Pages (gratuit, simple, sous le compte wolf3002)
- **Réception** : ntfy.sh (zéro compte, push natif via app)
- **Persistance** : localStorage côté client (5-10 MB, suffit largement)
- **Reproductibilité** : tout le code dans le repo, un seul fichier `index.html`

---

## 📞 Points de contact

- **Repo** : https://github.com/wolf3002/bilan-hanche-anais
- **Formulaire en ligne** : https://wolf3002.github.io/bilan-hanche-anais/
- **Réception ntfy (Valentin)** : https://ntfy.sh/bilan-anais-h4n7Kp9xR3vM
- **Topic ntfy à reconfigurer si compromis** : constante `NTFY_TOPIC` dans `index.html`
- **Toggle tests** : constante `ENABLE_TESTS_SECTION` dans `index.html`
