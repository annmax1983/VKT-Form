# vkt-form — Capture et auto-remplissage de formulaires

[English](../README.md) | [中文](README_zh.md) | [Español](README_es.md) | [Deutsch](README_de.md) | [日本語](README_ja.md) | Français

Une extension de navigateur qui sauvegarde des instantanés de formulaires web et les remplit automatiquement plus tard. Toutes les données stockées localement, aucun envoi cloud.

## Fonctionnalités

- **Collecte de formulaire en un clic** — Scannez et sauvegardez tous les champs de formulaire de n'importe quelle page
- **Détection approfondie** — Shadow DOM, iframes et éditeurs de texte riche inclus, ainsi que les étapes masquées d'un formulaire à plusieurs étapes
- **Auto-remplissage conscient du framework** — Écrit via le setter natif et le véritable pipeline d'édition du navigateur, donc les composants contrôlés de Vue / React / Angular mettent réellement à jour leur état
- **Vérification après écriture** — Chaque champ est relu après écriture ; les échecs sont signalés au lieu d'être ignorés
- **Calibration des champs** — Associez un champ à un élément de la page une fois, il sera toujours rempli ensuite
- **100 % local** — Toutes les données stockées dans `chrome.storage.local`, jamais envoyées
- **Export/Import** — Sauvegarde et restauration JSON
- **Niveau gratuit** — 5 instantanés, 20 remplissages/jour ; le Premium supprime toutes les limites

## Fonctionnement

1. Visitez une page contenant des formulaires, cliquez sur **Collecter** pour scanner et sauvegarder
2. Revenez sur la page plus tard, cliquez sur **Remplir** pour remplir automatiquement tous les champs
3. Gérez les instantanés dans le panneau latéral (remplir un instantané, mettre à jour, calibrer, supprimer)

## Correspondance des champs

Chaque champ enregistré est noté face à tous les champs de la page, et le meilleur candidat au-dessus du seuil de confiance l'emporte. En dessous, le champ est signalé comme introuvable plutôt que rempli dans la mauvaise case.

Signaux, par ordre approximatif de poids :

- Attributs `name`, `id` et `autocomplete`
- Texte du libellé, `aria-label`, `<label>` englobant, placeholder, texte proche
- Jeton sémantique (`username`, `phone`, `email`, `address`, …) en chinois et en anglais
- Chemin structurel et position ligne/colonne dans les conteneurs répétés (lignes de tableau)
- Ordre DOM, en dernier recours

La correspondance reposant sur des descripteurs et non sur des positions, un instantané fonctionne encore si le site renomme ses champs, réordonne le formulaire ou le sert depuis une autre URL.

## Remplissage

Chaque champ est écrit avec des stratégies progressives, et la valeur est relue après chaque tentative :

1. **Setter natif + événements** — écrit via le setter de `HTMLInputElement.prototype` et déclenche `beforeinput` / `input` / `change`. Passer par le prototype est ce qui fait réagir le suivi des modifications de React.
2. **Déclencheurs de validation** — `blur` / `focusout` pour les composants qui n'enregistrent qu'à la perte du focus.
3. **Pipeline d'édition réel** — `document.execCommand('insertText')` après focus et sélection ; les événements produits sont indiscernables d'une frappe réelle. C'est aussi ainsi que sont remplis les éditeurs de texte riche.
4. **Adaptateur de composants** — pour les selects en div (Element Plus, Ant Design, Arco, Naive UI, Vant, …), il ouvre la liste comme le ferait un utilisateur et clique sur l'option portant la valeur enregistrée.
5. **Mode débogueur** — désactivé par défaut ; il utilise le débogueur du navigateur pour produire des événements d'entrée de confiance sur les composants qui rejettent tout le reste. Chrome n'autorise pas la demande de cette permission à l'exécution : elle est accordée à l'installation, mais n'est utilisée qu'après activation du mode, et la connexion est coupée dès le remplissage terminé (une bannière de débogage s'affiche pendant ce temps).

Les champs toujours introuvables sont retentés pendant quelques secondes, ce qui permet aussi de remplir les formulaires rendus tardivement ou affichés conditionnellement.

## Calibration des champs

Les heuristiques couvrent la plupart des pages ; pour le reste, il y a la calibration. Ouvrez le panneau 🎯 d'un instantané, choisissez un champ, cliquez sur **Associer**, puis cliquez sur ce champ dans la page. L'association est enregistrée dans l'instantané et reste toujours prioritaire, quoi que fasse le site ensuite.

## Normalisation des URLs

Les instantanés sont indexés par URL normalisée :
- Paramètres de requête et fragments supprimés
- Domaine en minuscules
- Barres obliques finales normalisées
- Optionnel : conserver le hash route pour les applications SPA (bascule dans les Paramètres)

## Build

```bash
npm install
npm run build
```

Résultat : `publish/webformkeeper-v{version}.zip`

## Gratuit vs Premium

| | Gratuit | Premium |
|---|:---:|:---:|
| Instantanés | 5 max | Illimité |
| Remplissages par jour | 20 | Illimité |
| Export / Import JSON | — | ✅ |
| Support prioritaire | — | ✅ |

## Licence

Version gratuite : 5 instantanés, 20 remplissages/jour. La clé Premium débloque l'utilisation illimitée, l'export/import JSON et le support prioritaire.
