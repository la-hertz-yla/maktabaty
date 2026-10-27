# Maktabati — مكتبتي

Bibliothèque et espace d’annonces **de démonstration**, en arabe et RTL, réalisés avec React, Vite et CSS. Projet de portfolio indépendant de Dar Al Qamar.

## Lancer le projet

Node.js 22.12+ et npm requis ; Node 24 utilisé pour la validation.

```bash
git clone https://github.com/la-hertz-yla/maktabaty.git
cd maktabaty
npm ci
npm run dev
```

Ouvrir l’adresse indiquée par Vite dans le terminal.

```bash
npm run build
npm run preview
```

`dist/` contient le site de production. `preview` permet une vérification locale, ce n’est pas un serveur de production.

## Fonctionnalités opérationnelles

- Accueil, catalogue, fiche par livre, annonces, formulaire de création et profil : navigation par fragments `#/...`, compatible avec un hébergement statique sans réécriture d’URL.
- Huit livres arabes **inventés**, auteurs fictifs, descriptions cohérentes et couvertures originales dessinées en CSS. Aucun téléchargement de livre ou PDF.
- Recherche par titre ou auteur, tolérance aux signes diacritiques et aux variantes courantes de l’alif, filtres par cinq catégories, état sans résultat et réinitialisation.
- Ajout et retrait des favoris depuis les cartes, les fiches et le profil.
- Avis avec nom, note de 1 à 5 et commentaire. Les exemples initiaux sont clairement identifiés comme fictifs. Le profil affiche uniquement vos avis personnels.
- Annonce avec titre, auteur, catégorie, état, prix en MAD et description. Création, liste et suppression des annonces locales. Prix limité à 0,01–10000 MAD ; limites de longueur et rejet des champs texte composés uniquement d’espaces.
- Profil modifiable avec nom, présentation et avatar basé sur l’initiale du nom.
- Sauvegarde du profil, des favoris, avis et annonces dans `localStorage`, sous la clé `maktabati-demo-v1`. Les données restent après un rechargement, sous réserve que le stockage soit autorisé.
- Affichage RTL, contenu français/anglais isolé par `dir="auto"`, police arabe Noto Sans Arabic auto-hébergée, navigation mobile, focus clavier visibles, lien d’évitement et animations désactivables selon la préférence système.

## Limites à comprendre

Aucun backend ni compte authentifié. Les données restent **dans ce navigateur, pour cette origine** : elles ne sont pas synchronisées entre appareils, ne sont pas publiées et ne sont pas visibles par d’autres utilisateurs. Effacer les données du site ou quitter une session privée peut les supprimer. Un changement de domaine ou de port crée un espace de stockage distinct. Il n’existe aucun paiement, livraison, réservation ni mise en relation. Les prix des annonces servent seulement à tester le formulaire. Ne pas saisir de données sensibles.

Une erreur de stockage affiche un avertissement et les changements restent temporaires en mémoire. Un contenu local illisible ou mal formé est signalé ; des données initiales sûres sont alors utilisées. Les prochains changements peuvent remplacer ce contenu, si l’écriture est disponible.

Les avis initiaux, les livres et leurs auteurs sont fictifs et ne constituent ni des témoignages réels ni des publications téléchargeables. Aucun titre ne peut être acheté depuis une fiche.

## Modifier le contenu

- `src/data.js` : livres initiaux, catégories, états des annonces, profil initial et avis d’exemple. Pour un livre, donner un `id` unique, `title`, `author`, `category`, `description`, `subtitle`, `pages`, `year`, `color`, `accent` et `symbol` (`arch`, `leaf`, `orbit`, `wave`, `city`, `steps`).
- `src/components/UI.jsx` : couvertures, cartes, avis, champs et icônes SVG originales.
- `src/components/Layout.jsx` : navigation, avertissements de démonstration et pied de page.
- `src/pages/` : pages React ; `src/App.jsx` : navigation et mutations de l’état.
- `src/storage.js` : validation, lecture et écriture des données locales.
- `src/style.css` : palette, typographie, couvertures et points de rupture responsive.

Conserver les identifiants de livres pour éviter d’invalider les favoris et avis déjà enregistrés. Lors d’un changement de structure majeur, prévoir une migration du stockage. Pour réinitialiser la démonstration, supprimer uniquement la clé `maktabati-demo-v1` dans les outils de développement du navigateur, puis recharger la page.

Tous les visuels sont originaux (CSS et SVG), sans couverture ou photo empruntée. La police Noto Sans Arabic est distribuée sous la licence SIL Open Font License ; la licence est incluse dans `public/licenses/Noto-Sans-Arabic-OFL.txt` et dans le paquet `@fontsource/noto-sans-arabic`. Aucune requête vers Google Fonts n’est nécessaire. Les textes latins utilisent la police système de repli.

## Tests navigateur

```bash
npm ci
npx playwright install chromium
# Terminal 1
npm run dev -- --port 5174
# Terminal 2
npm test
```

Si Chromium est déjà installé sur Linux :

```bash
CHROMIUM_PATH=/usr/bin/chromium npm test
```

Pour une autre adresse : `TEST_URL=http://127.0.0.1:4174 npm test`. Pour enregistrer des captures, créer un dossier puis définir `SCREENSHOT_DIR=/chemin/absolu`.

La suite `tests/smoke.mjs` utilise un contexte navigateur isolé. Elle vérifie recherche, filtres, favoris, avis, annonces, validation du prix, profil, persistance après rechargement, suppression d’annonce, navigation mobile, lien d’évitement clavier, données de stockage corrompues et écriture indisponible. Elle vérifie les six pages à 360, 390, 768, 1280 et 1440 px sans débordement horizontal et relève les erreurs JavaScript. Les tests ne remplacent pas un audit d’accessibilité complet.

## Publier et partager

### GitHub Pages

Après envoi du code sur GitHub :

```bash
npm ci
npm run build
npx gh-pages -d dist
```

La commande `gh-pages` nécessite un accès GitHub en écriture et publie uniquement le build sur la branche `gh-pages`. Dans **Settings → Pages**, sélectionner **Deploy from a branch**, branche **gh-pages**, dossier **/ (root)**. Après publication réussie, GitHub fournira l’URL à partager, normalement `https://la-hertz-yla.github.io/maktabaty/`. Ce lien ne doit être considéré comme actif qu’après confirmation de GitHub. `base: './'` permet le fonctionnement dans ce sous-dossier.

### Netlify

```bash
npm run build
npx netlify-cli deploy --dir=dist
# Après vérification de la prépublication :
npx netlify-cli deploy --dir=dist --prod
```

Se connecter à son compte et sélectionner ou créer un site. Partager ensuite le lien HTTPS fourni par Netlify. Avec un dépôt connecté : commande `npm run build`, dossier `dist`, Node 24. Aucun secret n’est nécessaire dans le code.

La publication du dépôt GitHub ne publie pas automatiquement un site web. Aucun service de paiement ou d’authentification n’est activé par ces commandes.
