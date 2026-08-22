## 🛒 Système +REP (réputation vendeurs)

| Commande | Description | Accès |
|---|---|---|
| `+rep @vendeur <commentaire>` | Laisse un avis sur un vendeur après une transaction | Tous |
| `+infos @vendeur` | Fiche complète : total, historique numéroté de **tous** les +REP | Tous |
| `+suprep @vendeur <numéro>` | Supprime un +REP précis (numéro visible via `+infos`) | Auteur de l'avis ou Admin |
| `+toprep` | Leaderboard des vendeurs les mieux notés (top 15) | Tous |
| `!rep-reset @user` | Remet à zéro tous les +REP d'un utilisateur | Admin |

**Règles :**
- Impossible de se `+rep` soi-même (le vendeur ne peut pas se noter), ni de `+rep` un bot.
- `+infos` liste désormais **tout l'historique** des avis (auteur, commentaire, date), numéroté du plus récent au plus ancien. Au-delà de 40 avis, seuls les plus récents sont affichés (limite technique Discord).
- `+suprep @vendeur` sans numéro affiche la liste numérotée pour t'aider à choisir. `+suprep @vendeur 3` supprime l'avis n°3.
- Seul **l'auteur d'un avis** peut le supprimer lui-même (droit à l'erreur / retrait), sinon un **admin** peut supprimer n'importe quel avis (modération).
- `+toprep` classe tous les vendeurs du serveur par nombre total de +REP, médailles 🥇🥈🥉 pour le podium.
- Les données sont stockées dans `data/rep_data.json`.

**Exemples :**
```
+rep @Vendeur Colis reçu rapidement, produit conforme, merci !
+infos @Vendeur
+suprep @Vendeur
+suprep @Vendeur 3
+toprep
!rep-reset @Vendeur
```

---

## 🎬 Surveillance nouvelles vidéos TikTok

| Commande | Description | Accès |
|---|---|---|
| `!set-video-watch <pseudo_tiktok> <#channel>` | Définit le compte TikTok à surveiller et le salon de notification | Admin |
| `!video-watch-status` | Affiche la config actuelle (compte, salon, dernière vidéo connue) | Tous |
| `!video-watch-disable` | Coupe la surveillance | Admin |

**Fonctionnement :**
- Le bot vérifie le profil TikTok toutes les **5 minutes** (`CONFIG.VIDEO_CHECK_INTERVAL`, modifiable dans le code).
- Dès qu'une nouvelle vidéo est détectée (ID différent du dernier connu), un message avec lien direct est posté dans le salon configuré.
- Le tout premier check après configuration sert uniquement de référence (pas de notification), pour éviter de spam avec l'historique déjà existant.
- Comme pour la détection de live, c'est une détection par scraping HTML : si TikTok change la structure de ses pages, la détection peut temporairement casser (ouvrir une issue le cas échéant).

**Exemple :**
```
!set-video-watch crousgainz #nouvelles-videos
!video-watch-status
!video-watch-disable
```

---

## 🔍 Vérification améliorée

- Les caractères mal encodés (emojis cassés type `âš ï¸`, `â³`, `â❌`...) dans les embeds de vérification, de rating et de tournoi ont été corrigés.
- Le système de vérification détecte désormais les **doubles comptes potentiels** :
  - Un membre avec le **même avatar exact** (hash identique) qu'un autre membre déjà présent sur le serveur.
  - Un membre avec un **pseudo quasi identique** (normalisé : sans accents, sans chiffres finaux, casse ignorée) à un autre membre.
- Si une correspondance est trouvée, la demande de vérification affiche un nouveau champ **"👥 Comptes potentiellement liés"** listant les comptes suspects et la raison de la correspondance, et le message est marqué comme suspect (couleur rouge + `@here`).

> ⚠️ Discord ne donne pas accès aux adresses IP : cette détection reste une heuristique basée sur l'avatar et le pseudo, pas une preuve formelle.
