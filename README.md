# Abacus Comptabilité — Outil de visualisation

Application web statique pour comprendre et visualiser les données comptables issues d'**Abacus ERP** via AbaConnect SOAP/XML.

🌐 **Live demo** : [https://\<ton-username\>.github.io/abacus-compta](https://github.com)

---

## Fonctionnalités

### 📒 Journal
- Saisie d'écritures comptables (débit / crédit) avec autocomplétion du plan comptable suisse
- Gestion TVA : codes Abacus → comptes 1170 / 1171 / 2200 selon le contexte
- **Forme en T** : visualisation des comptes touchés par la dernière écriture
- **XML Abacus** : génération fidèle du XML `FibuBooking` (CollectiveInformation + SingleInformation + TaxData) pour toutes les écritures avec scrollbar
- Grand-livre par compte avec solde cumulé ligne par ligne

### 🗂 XML & Mapping
- Structure SOAP Abacus annotée et expliquée
- Pipeline Python : parsing XML → calcul `Σ Débit − Σ Crédit`
- **⚠️ Filtre Reversal** : les écritures `<apt:Type>Reversal</apt:Type>` sont extournées (annulées) et doivent être ignorées des calculs
- Visualisations : liquidités 10xx, dépenses/encaissements, charges par catégorie

### 🧾 TVA
- Taux suisses 2024+ : 8.1% / 2.6% / 3.8% / 0%
- Tous les codes TVA Abacus avec descriptions
- Calculatrice : montant HT → TVA → TTC → XML généré

---

## Logique TVA (comptes Abacus)

| Situation | Compte TVA |
|-----------|-----------|
| Achats avec compte `4xxx` au débit | **1170** TVA déductible charges |
| Autres achats (loyers, IT, véhicules…) | **1171** TVA déductible autres |
| Ventes avec compte `3xxx` ou `1100` | **2200** TVA collectée |

---

## Règle fondamentale

```
Solde = Σ Débit − Σ Crédit
```

> Ne jamais utiliser `SUM(Débit)` seul. Toujours la différence.  
> Ignorer les écritures `Type = Reversal` (extournées/annulées).

---

## Déploiement GitHub Pages

1. Fork ou clone ce repo
2. Aller dans **Settings → Pages**
3. Source : `Deploy from a branch` → branch `main` → dossier `/ (root)`
4. Sauvegarder — disponible en quelques minutes sur `https://<username>.github.io/abacus-compta`

---

## Structure du projet

```
abacus-compta/
├── index.html      ← application complète (single-file)
└── README.md
```

Application entièrement statique — aucun backend, aucune dépendance NPM.  
Seules dépendances CDN : IBM Plex fonts (Google Fonts) + Chart.js.

---

## Développement local

```bash
# Option 1 : ouvrir directement
open index.html

# Option 2 : serveur local simple
python3 -m http.server 8080
# puis ouvrir http://localhost:8080
```
