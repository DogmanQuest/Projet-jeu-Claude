# Centre d'Opérations — structure du projet

Jeu de gestion PMC. Un seul dépôt, deux versions distribuées :

## Les deux versions

| Version | Dossier | Mécanisme | Résultat |
|---|---|---|---|
| **Android (APK)** | `www/` | GitHub Actions (`build-apk.yml`) | APK téléchargeable dans l'onglet Actions → Artifacts |
| **Web / iPhone (PWA)** | `docs/` | GitHub Pages (Settings → Pages → branche main, dossier /docs) | Site installable via Safari → "Sur l'écran d'accueil" |

Les deux versions embarquent le même jeu (mêmes `bundle.js` / `output.css`).
La version web a en plus : `sw.js` (cache hors-ligne), `manifest.webmanifest`
et les icônes.

## Procédure de mise à jour (à chaque nouvelle version)

1. Récupérer le zip de mise à jour (contient `www/` et `docs/`)
2. Sur GitHub : **Add file → Upload files**, envoyer le contenu des deux
   dossiers en respectant l'arborescence (un seul commit suffit)
3. Vérifier que `www/bundle.js` ET `docs/bundle.js` affichent la même
   taille sur GitHub (~1.9 MB selon les versions) — si l'un des deux est
   plus petit, refaire son upload
4. Android : onglet Actions → attendre la coche verte → Artifacts →
   installer l'APK par-dessus (sauvegarde conservée, clé de signature stable)
5. Web/iPhone : rien à faire, Pages redéploie tout seul en 1-2 min ;
   les joueurs récupèrent la nouvelle version au prochain lancement
   (le numéro de version dans `sw.js` force le rafraîchissement du cache)

## Dossiers techniques (ne pas toucher sauf instruction)

- `.github/workflows/build-apk.yml` — chaîne de fabrication de l'APK
- `capacitor.config.json`, `package.json` — configuration Android
- `signing/debug.keystore` — clé de signature stable : c'est elle qui
  permet d'installer les mises à jour par-dessus sans désinstaller.
  **Ne jamais la supprimer ni la régénérer**, sinon tous les téléphones
  devront désinstaller/réinstaller (et perdront leur sauvegarde)
- `resources/icon.png`, `resources/splash.png` — icône et écran de
  démarrage de l'APK (l'icône web est dans `docs/`)

## Sauvegardes des joueurs

- Android : stockée dans l'app, survit aux mises à jour
- Web/iPhone : stockée dans Safari (localStorage), survit aux mises à
  jour mais saute si le joueur efface les données du site
