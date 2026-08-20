# WebFormKeeper
[English](../README.md) | [中文](README_zh.md) | [Español](README_es.md) | [Deutsch](README_de.md) | [日本語](README_ja.md) | Français

Extension de capture et remplissage automatique de formulaires web — Enregistrez en un clic, remplissez en un clic. Toutes les données stockées localement.

> Basé sur Chromium · Manifest V3 · Aucun suivi · Capture manuelle par l'utilisateur

---

## Pourquoi WebFormKeeper ?

Remplir les mêmes formulaires encore et encore est fastidieux. Avec WebFormKeeper, enregistrez une fois et remplissez quand vous voulez.

| Avantage | Détails |
|----------|---------|
| 🔒 **Confidentialité** | Toutes les données dans le navigateur. Aucun serveur, aucun upload, aucun suivi. |
| ⚡ **Un clic** | « Collecter » pour sauvegarder, « Remplir » pour restaurer. |
| 🧠 **Correspondance intelligente** | Attribut `name` en priorité, ordre DOM en secours, compatible Vue/React. |
| 💾 **Pas de perte de données** | Export/import JSON. |
| 🆓 **Gratuit** | 5 captures, 20 remplissages/jour. |
| 🌍 **6 langues** | Détection automatique de la langue du navigateur. |

---

## Fonctionnalités

| Fonction | Description |
|----------|-------------|
| 📋 **Capture de formulaires** | Bouton pour scanner tous les `input/select/textarea` de la page. |
| ⚡ **Remplissage intelligent** | Priorité : attribut `name`, secours : `domIndex + tagName + type`. |
| 🔄 **Compatible frameworks** | Événements `input`, `change`, `click`, compatible Vue/React. |
| 📊 **Gestion des quotas** | Gratuit : 5 captures, 20/jour. Premium : illimité. |
| 🔑 **Licence** | Activation Premium depuis la page des paramètres. |
| 📥📤 **Import/Export** | Sauvegarde et restauration JSON. |
| 🌐 **Normalisation d'URL** | Supprime requêtes/fragments, minuscules, hash optionnel (SPA). |
| 🌍 **Multi-langue** | English, 中文, 日本語, Español, Deutsch, Français. |

---

## Navigateurs compatibles

| Navigateur | Statut |
|------------|--------|
| Google Chrome | ✅ Complet |
| Microsoft Edge | ✅ Complet |
| Brave | ✅ Supporté |
| Opera | ✅ Supporté |
| Vivaldi | ✅ Supporté |
| Navigateurs Chromium | ✅ Supporté (Manifest V3) |

---

## Installation

### Mode développeur

1. Ouvrir la page des extensions :
   - **Chrome** : `chrome://extensions/`
   - **Edge** : `edge://extensions/`
2. Activer le **Mode développeur**
3. **Charger l'extension non empaquetée** → sélectionner le dossier `WebFormKeeper`
4. L'icône apparaît dans la barre d'outils

---

## Utilisation

### Enregistrer une capture de formulaire

1. Visiter une page avec un formulaire
2. Cliquer sur l'icône dans la barre
3. Cliquer sur **🔄 Collecter**
4. Les champs sont scannés et enregistrés

### Remplissage automatique

**Méthode A : Correspondance page actuelle**
1. Retourner sur la page enregistrée
2. Cliquer sur **⚡ Remplir**
3. Remplissage automatique

**Méthode B : Remplir une capture spécifique**
1. Trouver l'enregistrement dans la liste
2. Cliquer sur **Remplir celle-ci**
3. Remplissage forcé avec cette capture

---

## Confidentialité

- ✅ **Aucun upload** — Stockage dans `chrome.storage.local`
- ✅ **Capture manuelle** — Aucun scan automatique
- ✅ **Aucun suivi** — Pas de télémétrie ni d'appels distants
- ✅ **Permissions minimales** — Uniquement `storage` et `activeTab`

---

## Permissions

| Permission | But |
|------------|-----|
| `storage` | Stockage local des captures et paramètres |
| `activeTab` | Accès à l'onglet actuel uniquement au clic |

---

## ❤️ Soutien

Si WebFormKeeper vous aide, n'hésitez pas à nous soutenir !

**[👉 Soutenir WebFormKeeper](https://annmax1983.github.io/WebFormKeeper/)**
