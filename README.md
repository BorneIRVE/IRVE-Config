# ⚡ IRVE Configurateur — Guide de déploiement

## Architecture

```
GitHub Repository
├── index.html          ← Le configurateur (ton site web)
├── tarifs.json         ← Tarifs mis à jour automatiquement
├── update_tarifs.py    ← Script de mise à jour
└── .github/
    └── workflows/
        └── update-tarifs.yml  ← Robot GitHub Actions (tourne 1x/mois)
```

**Flux automatique :**
```
Chaque 1er du mois → GitHub Actions → Anthropic API (web search) → tarifs.json mis à jour → ton site affiche les nouveaux tarifs
```

**Coût mensuel estimé : ~0,03 € (un seul appel API Anthropic/mois)**

---

## Déploiement pas à pas

### Étape 1 — Créer un compte GitHub
1. Va sur [github.com](https://github.com) → "Sign up"
2. Crée un compte gratuit

### Étape 2 — Créer le repository
1. Clique sur **"New repository"** (bouton vert)
2. Nom : `irve-configurateur` (ou ce que tu veux)
3. Coche **"Public"** (nécessaire pour GitHub Pages gratuit)
4. Clique **"Create repository"**

### Étape 3 — Uploader les fichiers
1. Dans ton nouveau repository, clique **"uploading an existing file"**
2. Glisse-dépose ces 4 fichiers :
   - `index.html`
   - `tarifs.json`
   - `update_tarifs.py`
3. Pour le dossier `.github/workflows/` :
   - Clique **"Add file" → "Create new file"**
   - Nomme-le `.github/workflows/update-tarifs.yml`
   - Colle le contenu du fichier
4. Clique **"Commit changes"**

### Étape 4 — Activer GitHub Pages (hébergement gratuit)
1. Dans ton repository → onglet **"Settings"**
2. Menu gauche → **"Pages"**
3. Source : **"Deploy from a branch"**
4. Branch : **"main"** → dossier **"/ (root)"**
5. Clique **"Save"**
6. Ton site sera disponible sur : `https://TON-PSEUDO.github.io/irve-configurateur/`

### Étape 5 — Ajouter la clé API Anthropic (secret sécurisé)
1. Va sur [console.anthropic.com](https://console.anthropic.com)
2. Crée un compte → **"API Keys"** → **"Create Key"**
3. Copie la clé (commence par `sk-ant-...`)
4. Dans ton repository GitHub → **"Settings"** → **"Secrets and variables"** → **"Actions"**
5. Clique **"New repository secret"**
   - Name : `ANTHROPIC_API_KEY`
   - Secret : colle ta clé API
6. Clique **"Add secret"**

### Étape 6 — Tester la mise à jour
1. Dans ton repository → onglet **"Actions"**
2. Clique sur **"Mise à jour automatique des tarifs"**
3. Clique **"Run workflow"** → **"Run workflow"**
4. Attends ~30 secondes → tu verras les logs en direct
5. Si ✅ vert : tout fonctionne, `tarifs.json` a été mis à jour

---

## Mise à jour manuelle si besoin
1. Onglet **"Actions"** → **"Mise à jour automatique des tarifs"**
2. **"Run workflow"** → **"Run workflow"**
3. C'est tout.

## Modifier les tarifs manuellement (urgence)
1. Dans le repository → clique sur `tarifs.json`
2. Clique sur l'icône crayon ✏️
3. Modifie les valeurs
4. **"Commit changes"**

---

## Intégration sur ton site d'entreprise

Une fois ton site créé (Webflow, WordPress, etc.), tu as 2 options :

**Option A — Utiliser directement GitHub Pages**
Ton configurateur est déjà en ligne sur `github.io`, tu mets juste un lien ou un iframe depuis ton site.

**Option B — Héberger le HTML sur ton site**
Copie `index.html` sur ton hébergeur. Le fichier fetchera toujours `tarifs.json` depuis GitHub :
```
https://raw.githubusercontent.com/TON-PSEUDO/irve-configurateur/main/tarifs.json
```
Remplace `TON-PSEUDO` par ton nom d'utilisateur GitHub dans `index.html`.

---

## Fréquence des mises à jour

| Événement | Fréquence |
|-----------|-----------|
| Mise à jour automatique | 1er de chaque mois |
| Révisions CRE (tarifs réglementés) | 1er février et 1er août |
| Mise à jour manuelle possible | À tout moment via "Run workflow" |

---

## Support

En cas de problème avec le robot :
1. Onglet **"Actions"** → clique sur le run en erreur (croix rouge)
2. Lis les logs → l'erreur est indiquée clairement
3. Les tarifs **ne sont jamais effacés** en cas d'erreur (le script conserve les valeurs actuelles)
