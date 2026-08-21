## 🛒 Système +REP (réputation vendeurs)

| Commande | Description | Accès |
|---|---|---|
| `+rep @vendeur <commentaire>` | Laisse un avis sur un vendeur après une transaction | Tous |
| `+infos @vendeur` | Affiche la fiche complète d'un vendeur | Tous |
| `!rep-reset @user` | Remet à zéro les +REP d'un utilisateur | Admin |

**Règles :**
- Impossible de se `+rep` soi-même (le vendeur ne peut pas se noter).
- Impossible de `+rep` un bot.
- `+infos` affiche : pseudo, photo de profil, nombre total de +REP, qui a laissé un +REP (mentions), et la date du dernier +REP.
- Les données sont stockées dans `data/rep_data.json`.

**Exemples :**
```
+rep @Vendeur Colis reçu rapidement, produit conforme, merci !
+infos @Vendeur
!rep-reset @Vendeur
```

---

## 🔍 Vérification améliorée

- Les caractères mal encodés (emojis cassés type `âš ï¸`, `â³`, `â❌`...) dans les embeds de vérification, de rating et de tournoi ont été corrigés.
- Le système de vérification détecte désormais les **doubles comptes potentiels** :
  - Un membre avec le **même avatar exact** (hash identique) qu'un autre membre déjà présent sur le serveur.
  - Un membre avec un **pseudo quasi identique** (normalisé : sans accents, sans chiffres finaux, casse ignorée) à un autre membre.
- Si une correspondance est trouvée, la demande de vérification affiche un nouveau champ **"👥 Comptes potentiellement liés"** listant les comptes suspects et la raison de la correspondance, et le message est marqué comme suspect (couleur rouge + `@here`).

> ⚠️ Discord ne donne pas accès aux adresses IP : cette détection reste une heuristique basée sur l'avatar et le pseudo, pas une preuve formelle.
