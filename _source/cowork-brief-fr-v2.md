# BRIEF COWORK — Projet Noweb. (v2)

> Owner : Marwan (Perth, Australie)
> Mission : Mettre en ligne ce soir un business de web design prêt pour lancer Meta Ads.
> Objectif : Première pub live dans 4-6 heures.

---

## CONTEXTE — à lire en premier

Je lance **Noweb.**, un studio de web design qui cible **toutes les entreprises australiennes** qui ont besoin d'un site web — PME, indépendants, professions libérales, e-commerce, restaurants, services, retail, immobilier, santé/beauté, fitness, professionnels (avocats, comptables, consultants), etc. Pas de discrimination par taille, n'importe quelle entreprise qui veut un site pro est cliente potentielle.

L'offre : sites web premium custom à $990 AUD, livrés en 7 jours, avec abonnement optionnel de gestion à **$60/semaine**. Acquisition 100% Meta Ads (Instagram + Facebook), pas d'appel client, checkout Stripe direct depuis la landing page.

Ce que j'ai déjà :
- ABN actif en Australie
- Domaine `noweb.com.au` acheté chez VentraIP (DNS pas encore configuré)
- 6 fichiers générés par Claude (3 HTML + 3 markdown manuels)
- Compte Gmail `nowebcontact@gmail.com` pour les opérations
- Capital sous $1000 AUD, dispo à 100%
- Anglais intermédiaire (donc communication écrite uniquement avec les clients, pas d'appels)

La stratégie produit est verrouillée. Les 6 fichiers sont finaux. Reste juste l'exécution.

---

## LES 6 FICHIERS (déjà générés, dans /mnt/user-data/outputs/ depuis ma session Claude)

1. **saltwood-kitchen.html** — pièce de portfolio, démo de site café. Sera hébergé sur `noweb.com.au/work/saltwood`.
2. **cove-landing.html** — landing page de vente principale. Sera hébergée sur `noweb.com.au` (root index). À RENOMMER EN `index.html`.
3. **cove-brief.html** — formulaire de brief client post-paiement. Sera hébergé sur `noweb.com.au/brief`. À RENOMMER EN `brief.html`.
4. **cove-playbook.md** — manuel des opérations (référence, pas déployé).
5. **cove-meta-ads-pack.md** — briefs créatifs pour les 6 ads (référence, pas déployé).
6. **cove-launch-manual.md** — checklist pre-launch (référence, pas déployé).

---

## CE QUE J'AI BESOIN QUE TU FASSES CE SOIR — dans l'ordre

### TÂCHE 1 — Création de la structure de dossier

Crée cette structure exacte sur mon bureau (ou un endroit propre) :

```
noweb-site/
├── index.html              (renommé depuis cove-landing.html)
├── brief.html              (renommé depuis cove-brief.html)
└── work/
    └── saltwood.html       (renommé depuis saltwood-kitchen.html)
```

### TÂCHE 2 — Find & Replace dans les 3 fichiers HTML

Les fichiers ont été générés avec la marque placeholder "Cove." qui doit devenir "Noweb." ET avec un tarif Care de $40/semaine qui doit passer à $60/semaine.

**Applique ces remplacements globalement dans les 3 fichiers HTML, dans cet ordre exact :**

#### Phase 1 — Branding

| Cherche | Remplace par |
|---|---|
| `Cove.` | `Noweb.` |
| `Cove<span>.</span>` | `Noweb<span>.</span>` |
| `Cove Studio` | `Noweb` |
| `cove studio` | `noweb` |
| `covestudio.com.au` | `noweb.com.au` |
| `hello@covestudio.com.au` | `hello@noweb.com.au` |
| `Cove Pixel` | `Noweb Pixel` |
| `from cove-` | `from noweb-` |

**Attention :** "Cove" tout seul (sans le point) apparaît à certains endroits dans des descriptifs où il faut le laisser tel quel. Remplace UNIQUEMENT les chaînes exactes du tableau ci-dessus pour éviter les faux positifs.

#### Phase 2 — Tarif Care subscription ($40/wk → $60/wk)

| Cherche | Remplace par |
|---|---|
| `$40/week` | `$60/week` |
| `$40/wk` | `$60/wk` |
| `+$40/week` | `+$60/week` |
| `value: 40,` | `value: 60,` |
| `$40 if Care` | `$60 if Care` |
| `at $40/week` | `at $60/week` |

**Attention spéciale pour ce tarif :** vérifie aussi dans le code JavaScript du fichier `index.html` ces lignes spécifiques (autour du toggle upsell) :

```javascript
recurringDisplay.innerHTML = '$40 <em>/week</em>';
checkoutBtn.innerHTML = 'Secure your spot — Pay $990 + $40/wk →';
```

Doivent devenir :

```javascript
recurringDisplay.innerHTML = '$60 <em>/week</em>';
checkoutBtn.innerHTML = 'Secure your spot — Pay $990 + $60/wk →';
```

Vérifie aussi les sections FAQ et le toggle upsell dans le HTML qui mentionnent `$40/week` dans le visible — tout doit passer à `$60/week`.

### TÂCHE 3 — Numéro de téléphone WhatsApp

Dans `index.html`, cherche ce lien WhatsApp :
```
https://wa.me/61400000000
```
Et remplace par mon vrai numéro WhatsApp — **demande-moi le numéro avant de faire cette tâche**, n'invente pas.

### TÂCHE 4 — Vérification de l'intégrité HTML

Après tous les find/replace, ouvre chacun des 3 fichiers en local dans un navigateur et vérifie :
- La page se charge sans erreurs dans la console
- Toutes les sections s'affichent correctement
- Aucun layout cassé par un remplacement de texte accidentel
- Le toggle upsell sur la landing affiche bien $60/week (pas $40)
- La vue mobile fonctionne (DevTools mode responsive)

Si quelque chose paraît cassé, signale-le avant le déploiement.

### TÂCHE 5 — Déploiement Netlify (drag-and-drop)

Le chemin le plus simple : déployer le dossier `noweb-site/` complet sur Netlify via drag-and-drop.

1. Va sur https://app.netlify.com/drop
2. Glisse le dossier `noweb-site/` dans la zone de drop
3. Attends ~30 secondes pour le déploiement
4. Récupère l'URL temporaire que Netlify donne (par ex. `something-1234.netlify.app`)
5. Teste l'URL dans un navigateur, vérifie que les 3 pages fonctionnent :
   - `https://[temp-url].netlify.app/` → landing page
   - `https://[temp-url].netlify.app/brief.html` → formulaire de brief
   - `https://[temp-url].netlify.app/work/saltwood.html` → portfolio

**Note :** La création du compte Netlify peut nécessiter mon input (confirmation email). Mets en pause et demande-moi si besoin.

### TÂCHE 6 — Connexion du domaine custom `noweb.com.au`

Une fois le site live sur l'URL temporaire :
1. Dans le dashboard Netlify → Domain Settings → Add custom domain → `noweb.com.au`
2. Netlify donne les instructions DNS (généralement : A record vers le load balancer Netlify + CNAME pour www)
3. Documente ces records DNS exactement

Ensuite j'irai me connecter sur VentraIP moi-même pour ajouter ces records DNS (auth requise — tu peux pas faire cette partie).

Après propagation DNS (~15 min à quelques heures), vérifie :
- `https://noweb.com.au` charge la landing
- Le certificat HTTPS est auto-provisionné par Netlify
- Tous les liens internes fonctionnent

---

## CE QUE TU NE PEUX PAS FAIRE (je m'en occupe moi-même)

Tout ça nécessite mon authentification / carte / décisions personnelles. N'essaie pas :

- Création compte Stripe Australia + Payment Links (vérification ID requise)
- Création compte Web3Forms (auth Gmail nécessaire)
- Setup Meta Business Manager + Pixel (login Facebook nécessaire)
- Inscription GST sur ATO (auth myGov nécessaire)
- Modifications DNS sur VentraIP (login registrar nécessaire)
- Setup Cloudflare Email Routing (login Cloudflare nécessaire)
- Production des 6 creatives Canva (besoin de jugement créatif + compte Canva)
- Génération Privacy Policy / Terms of Service (Termly ou similaire, mon compte)

**Ne sois pas malin en cherchant à contourner.** C'est verrouillé par auth pour de bonnes raisons de sécurité. Liste ce qu'il faut et laisse-moi gérer en parallèle pendant que tu fais le travail sur les fichiers.

---

## LIVRABLES — ce que j'attends de toi

À la fin de ta session, je veux :

1. ✅ Dossier `noweb-site/` créé avec la bonne structure
2. ✅ Les 3 fichiers HTML renommés et tous les find&replace appliqués proprement (branding + tarif $60)
3. ✅ Vérification locale faite (toggle upsell affiche $60, pas $40)
4. ✅ Site déployé sur Netlify, URL temporaire qui fonctionne
5. 📋 Records DNS documentés pour VentraIP (je les ajouterai moi-même)
6. 📋 Liste des anomalies, layouts cassés, ou points que tu as flaggés

---

## RÈGLES OPÉRATIONNELLES

- **N'invente jamais de valeurs placeholder.** Si tu rencontres un placeholder type `REPLACE_WITH_*` (comme `REPLACE_WITH_WEB3FORMS_ACCESS_KEY` ou `STRIPE_LINK_*`), laisse-le tel quel. Je remplirai ces valeurs une fois mes comptes créés.
- **Ne modifie pas le contenu au-delà du Find & Replace.** Pas d'« améliorations » de copy, pas de reformulation, pas d'ajustements de design. Les fichiers sont validés tels quels (sauf branding et tarif Care comme indiqué).
- **Si tu rencontres un blocage qui nécessite auth, arrête-toi et demande.** Ne gaspille pas de cycles à essayer des alternatives.
- **Confirme avant de déployer.** Montre-moi la structure de fichiers et un résumé des changements avant de push sur Netlify.

---

## PRIORITÉ

C'est mode sprint. Vitesse > perfection sur le travail fichiers. L'objectif : pendant que je setup Stripe + Meta en parallèle, toi tu livres un site déployé sur une URL temporaire que je vais brancher à mon domaine.

C'est parti.

— Marwan
