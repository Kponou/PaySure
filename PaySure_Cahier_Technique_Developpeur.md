# PAY SURE — CAHIER TECHNIQUE DÉVELOPPEUR
**Document de déploiement officiel**
Propriétaire : Gilles KPONOU | Version 1.0 | Avril 2026

---

## 1. PRÉSENTATION DU PROJET

**Pay Sure** est une application de recouvrement et de gestion de paiements (loyers, frais scolaires, salaires, redevances) avec messagerie intégrée, gestion multi-entreprises, et portail de maintenance super-admin.

Le code source est un composant React unique (`PaySure.jsx`) à transformer en application déployable.

---

## 2. FICHIERS FOURNIS PAR LE PROPRIÉTAIRE

| Fichier | Description |
|---|---|
| `PaySure.jsx` | Code source React complet de l'application |
| Ce document | Instructions de déploiement |

Le fichier `PaySure.jsx` contient toute la logique, les traductions (10 langues), le design et les données de démonstration. Aucune dépendance externe n'est requise hormis React.

---

## 3. DÉPLOIEMENT OPTION A — PWA (Recommandée)
### Application Web Installable sur Android + ordinateur

**Durée estimée : 2 à 4 heures**
**Coût hébergement : 0 à 5 USD/mois**

---

### Étape 1 — Environnement requis sur le PC du développeur

```
Node.js v18 ou supérieur → https://nodejs.org
npm (inclus avec Node.js)
Git → https://git-scm.com
Un compte Vercel (gratuit) → https://vercel.com
```

---

### Étape 2 — Créer le projet React

```bash
# Dans le terminal, exécuter dans l'ordre :
npx create-react-app paysure
cd paysure

# Supprimer le contenu par défaut
rm src/App.js src/App.css src/App.test.js src/logo.svg
```

---

### Étape 3 — Placer le fichier source

```bash
# Copier PaySure.jsx dans le dossier src/
cp /chemin/vers/PaySure.jsx src/PaySure.jsx
```

Modifier `src/index.js` pour qu'il pointe vers le bon composant :

```javascript
// Remplacer tout le contenu de src/index.js par ceci :
import React from 'react';
import ReactDOM from 'react-dom/client';
import PaySureApp from './PaySure';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<React.StrictMode><PaySureApp /></React.StrictMode>);
```

---

### Étape 4 — Configurer la PWA (pour installation sur Android)

Modifier `public/manifest.json` :

```json
{
  "short_name": "Pay Sure",
  "name": "Pay Sure — Collect. Track. Grow.",
  "icons": [
    {
      "src": "favicon.ico",
      "sizes": "64x64 32x32 24x24 16x16",
      "type": "image/x-icon"
    },
    {
      "src": "logo192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "logo512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
  "start_url": ".",
  "display": "standalone",
  "theme_color": "#050C18",
  "background_color": "#050C18",
  "description": "Payment recovery and management platform"
}
```

Modifier `public/index.html` — remplacer la balise `<title>` :

```html
<title>Pay Sure</title>
```

---

### Étape 5 — Activer le Service Worker (installation offline)

Dans `src/index.js`, ajouter à la fin :

```javascript
import * as serviceWorkerRegistration from './serviceWorkerRegistration';
serviceWorkerRegistration.register();
```

---

### Étape 6 — Tester en local

```bash
npm start
# Ouvrir http://localhost:3000 dans le navigateur
# Tester toutes les fonctions : login, paiements, messages, etc.
```

---

### Étape 7 — Générer le build de production

```bash
npm run build
# Un dossier /build/ est créé — c'est ce dossier qui sera mis en ligne
```

---

### Étape 8 — Déployer sur Vercel (gratuit)

```bash
# Installer l'outil Vercel
npm install -g vercel

# Déployer
vercel

# Suivre les instructions à l'écran :
# - Confirm project name: paysure (ou au choix)
# - Select framework: Create React App
# - Deploy

# L'URL de l'application sera fournie, par exemple :
# https://paysure.vercel.app
```

**Alternative : déploiement Netlify**
```bash
npm install -g netlify-cli
netlify deploy --prod --dir=build
```

---

### Étape 9 — Installation sur Android

1. Ouvrir l'URL de l'application dans **Chrome sur Android**
2. Appuyer sur les **3 points** en haut à droite
3. Sélectionner **"Ajouter à l'écran d'accueil"** (ou "Installer l'application")
4. L'icône Pay Sure apparaît sur l'écran d'accueil comme une vraie app

---

## 4. DÉPLOIEMENT OPTION B — APK ANDROID NATIF
### Fichier .apk installable directement (sans Play Store)

**Durée estimée : 6 à 10 heures**
**Outil : Capacitor (Ionic Framework)**

---

### Prérequis supplémentaires

```
Android Studio → https://developer.android.com/studio
Java JDK 17 → https://adoptium.net
Variables d'environnement ANDROID_HOME et JAVA_HOME configurées
```

---

### Étape 1 — Partir du build de l'Option A (obligatoire)

Effectuer d'abord les étapes 1 à 7 de l'Option A pour avoir le dossier `/build/`.

---

### Étape 2 — Installer Capacitor

```bash
# Dans le dossier du projet React
npm install @capacitor/core @capacitor/cli @capacitor/android

# Initialiser Capacitor
npx cap init "Pay Sure" "app.paysure.mobile" --web-dir=build
```

---

### Étape 3 — Ajouter la plateforme Android

```bash
npx cap add android
```

---

### Étape 4 — Synchroniser le code

```bash
# À chaque modification du code, exécuter dans l'ordre :
npm run build
npx cap sync android
```

---

### Étape 5 — Configurer l'identité de l'application

Modifier `android/app/src/main/res/values/strings.xml` :

```xml
<resources>
    <string name="app_name">Pay Sure</string>
    <string name="title_activity_main">Pay Sure</string>
    <string name="package_name">app.paysure.mobile</string>
    <string name="custom_url_scheme">app.paysure.mobile</string>
</resources>
```

---

### Étape 6 — Personnaliser les couleurs Android

Modifier `android/app/src/main/res/values/colors.xml` :

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="colorPrimary">#050C18</color>
    <color name="colorPrimaryDark">#050C18</color>
    <color name="colorAccent">#F0A500</color>
</resources>
```

---

### Étape 7 — Générer l'APK

```bash
# Ouvrir Android Studio
npx cap open android

# Dans Android Studio :
# Build → Build Bundle(s) / APK(s) → Build APK(s)
# Le fichier APK se trouve dans :
# android/app/build/outputs/apk/debug/app-debug.apk
```

---

### Étape 8 — Installer sur un téléphone Android

```bash
# Via câble USB (mode développeur activé sur le téléphone)
adb install android/app/build/outputs/apk/debug/app-debug.apk

# Ou copier le fichier .apk sur le téléphone et l'ouvrir manuellement
# (Activer "Sources inconnues" dans les paramètres Android)
```

---

## 5. OPTION C — PUBLICATION GOOGLE PLAY STORE

### Prérequis
- Compte développeur Google Play : **25 USD** (unique)
- APK signé (voir ci-dessous)

### Générer un APK signé pour le Play Store

```bash
# Dans Android Studio :
# Build → Generate Signed Bundle / APK
# → Créer un keystore (conserver précieusement ce fichier .jks)
# → Remplir les informations (nom, organisation, pays)
# → Générer le bundle .aab

# Uploader le .aab sur Google Play Console
# https://play.google.com/console
```

---

## 6. ACCÈS ADMINISTRATEUR — INFORMATIONS SENSIBLES

> ⚠️ **À NE PAS PARTAGER avec des tiers. Réservé au propriétaire et au développeur de confiance.**

### Compte Super Admin (propriétaire plateforme)

| Paramètre | Valeur |
|---|---|
| Email de connexion | `admin@paysure.app` |
| Rôle | Super Admin — Gilles KPONOU |
| Compte retrait Mobile Money | `+229 0196308678` |
| Accès | Portail Maintenance + toutes les fonctions |

### Comptes de démonstration (à modifier en production)

| Email | Rôle |
|---|---|
| `admin@paysure.app` | Super Admin / Propriétaire |
| `admin@co.com` | Admin d'entreprise |
| `user@co.com` | Membre simple |

> **En production :** connecter l'application à une base de données réelle (Supabase, Firebase ou PostgreSQL) et remplacer les données de démonstration hardcodées par de vraies API.

---

## 7. FONCTIONNALITÉS IMPLÉMENTÉES

| Fonctionnalité | Statut |
|---|---|
| Tableau de bord avec statistiques | ✅ Complet |
| Gestion des paiements (loyer, scolarité, salaire, redevances) | ✅ Complet |
| Filtres par statut et par type | ✅ Complet |
| Ajout de nouveau paiement | ✅ Complet |
| Marquer comme payé / Relance | ✅ Complet |
| Messagerie individuelle | ✅ Complet |
| Messagerie de groupe | ✅ Complet |
| Notifications / Alertes | ✅ Complet |
| Gestion des membres | ✅ Complet |
| Code d'accès entreprise | ✅ Complet |
| Retrait Mobile Money (+229 0196308678) | ✅ Complet |
| Retrait par virement bancaire | ✅ Complet |
| Retrait par carte bancaire | ✅ Complet |
| Portail Maintenance Super Admin | ✅ Complet |
| 10 langues (EN, FR, DE, RU, ES, PT, AR, YO, HA, FON) | ✅ Complet |
| Interface responsive (Android + ordinateur) | ✅ Complet |
| Isolation par entreprise (code d'accès) | ✅ Complet |
| Création / Rejoindre une organisation | ✅ Complet |

---

## 8. PROCHAINES ÉTAPES RECOMMANDÉES (Phase 2)

Pour une version production complète, le développeur devra ajouter :

1. **Base de données** — Supabase (PostgreSQL) ou Firebase (recommandé pour mobile)
2. **Authentification sécurisée** — Supabase Auth ou Firebase Auth (email + OTP téléphone)
3. **API de paiement mobile** — Intégration Wave, MTN Mobile Money, Moov Money via leurs API officielles
4. **Notifications push** — Firebase Cloud Messaging (FCM) pour alertes temps réel
5. **SMS de relance** — Twilio ou Africa's Talking API
6. **Multi-tenancy réelle** — isolation complète des données par entreprise en base
7. **Icône et splash screen personnalisés** — logo Pay Sure créé par un designer

---

## 9. CONTACTS ET LIVRAISON

Le développeur doit livrer au propriétaire :

- [ ] L'URL de l'application déployée (PWA)
- [ ] Le fichier `.apk` installable (Option B)
- [ ] Le fichier keystore `.jks` signé (si Play Store)
- [ ] Les identifiants du compte Vercel / Netlify
- [ ] La documentation des modifications apportées

**Propriétaire du projet :** Gilles KPONOU
**Compte commission plateforme :** Mobile Money +229 0196308678

---

*Document généré par Pay Sure — Confidential*
*Toute reproduction non autorisée est interdite.*
