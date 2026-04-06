# 1. Cartographie fonctionnelle 

| # | Fonctionnalité | Priorité | Motif |
|---|---|---|---|
| F1 | Gestion compte & abonnement | 🔴 P1 | Risque légal + monétisation |
| F3 | Création / édition du CV | 🔴 P1 | Cœur de valeur produit |
| F7 | Téléchargement PDF | 🔴 P1 | Point de conversion final |
| F2 | Sélection de template | 🟠 P2 | Bloquant si cassé, mais contournable |
| F4 | Assistance IA | 🟠 P2 | Fonctionnalité récente, risque de régression |
| F8 | Lettre de motivation | 🟠 P2 | Valeur ajoutée mais non critique |
| F5 | Personnalisation visuelle | 🟡 P3 | Cosmétique, impact limité |
| F6 | Multilingue | 🟡 P3 | Usage minoritaire, risque faible |
| F9 | Optimisation ATS | 🟡 P3 | Pas d'interaction directe testable |

---

# 2. Matrice de risques

### Méthode de scoring
Score de risque = Impact métier (1→3) × Probabilité de bug (1→3)
Plage : 1 (négligeable) → 9 (critique)

**Impact métier**
| Score | Signification |
|---|---|
| 3 | Bloquant — perte de revenus, donnée perdue, incident légal |
| 2 | Dégradant — expérience altérée, contournement possible |
| 1 | Mineur — cosmétique, usage marginal |

**Probabilité de bug**
| Score | Signification |
|---|---|
| 3 | Élevée — fonctionnalité récente, complexe ou signalée utilisateurs |
| 2 | Moyenne — stable mais dépendances externes |
| 1 | Faible — simple, peu de changements récents |


### Résultats

| # | Fonctionnalité | Impact (1-3) | Probabilité (1-3) | Score (I×P) | Niveau de risque | Priorité |
|---|---|:---:|:---:|:---:|---|:---:|
| F1 | Gestion compte & abonnement | 3 | 3 | **9** | 🔴 Critique | P1 |
| F7 | Téléchargement PDF | 3 | 3 | **9** | 🔴 Critique | P1 |
| F3 | Création / édition du CV | 3 | 2 | **6** | 🟠 Élevé | P1 |
| F4 | Assistance IA | 2 | 3 | **6** | 🟠 Élevé | P2 |
| F8 | Lettre de motivation | 2 | 2 | **4** | 🟡 Modéré | P2 |
| F2 | Sélection de template | 2 | 2 | **4** | 🟡 Modéré | P2 |
| F5 | Personnalisation visuelle | 1 | 2 | **2** | 🟢 Faible | P3 |
| F6 | Multilingue | 1 | 2 | **2** | 🟢 Faible | P3 |
| F9 | Optimisation ATS | 1 | 1 | **1** | 🟢 Négligeable | P3 |

---

# 3. Plan de test priorisé

# Plan de test priorisé — MakeMy CV

## Contexte et contraintes

| Paramètre | Valeur |
|---|---|
| Environnement | Production (déploiement canary) |
| Fenêtre de test pré-déploiement | Matin, avant 9h |
| Fenêtre de surveillance post-déploiement | 3h à 20% du trafic |
| Couverture de test existante | Aucune |
| Approche | Risk-Based Testing (ISTQB) |
| Critère de priorisation | Score de risque I×P issu de la matrice |


## Principe de priorisation

En l'absence de suite de tests existante, le plan suit trois règles :

1. **Le score de risque prime** — les parcours à score 9 sont testés
   en premier, sans exception.
2. **Le parcours utilisateur complet avant les cas limites** — on
   valide d'abord le "happy path" (chemin nominal), puis les cas
   d'erreur et les cas limites.
3. **L'indépendance des parcours** — chaque bloc peut être exécuté
   et reporté séparément pour faciliter le go/no-go partiel.


## Vue d'ensemble du plan

| Bloc | Fonctionnalité | Score | Parcours couverts | Durée estimée |
|---|---|:---:|:---:|---:|
| B1 | Gestion compte & abonnement | 9 | 4 | 45 min |
| B2 | Téléchargement PDF | 9 | 3 | 30 min |
| B3 | Création / édition du CV | 6 | 4 | 40 min |
| B4 | Assistance IA | 6 | 3 | 25 min |
| B5 | Lettre de motivation | 4 | 2 | 20 min |
| B6 | Sélection de template | 4 | 2 | 15 min |
| **Total** | | | **18** | **~3h** |

> Les blocs B5 et B6 ne sont exécutés que si B1, B2 et B3 sont validés sans défaut bloquant 
> En cas de défaut P1, les ressources sont redirigées vers la correction et la re-vérification 


## Détail des parcours par bloc

### B1 — Gestion compte & abonnement
**Score de risque : 9 | Priorité : P1**

Justification : point d'entrée de toute la valeur produit,
cœur de la monétisation, et zone déjà signalée comme défaillante
par des utilisateurs réels (souscription non consentie à 27,95€/mois
relevée sur Trustpilot).

| ID | Parcours | Type | Critère de succès |
|---|---|---|---|
| B1-P1 | Inscription avec email valide | Happy path | Compte créé, email de confirmation reçu |
| B1-P2 | Connexion avec identifiants valides | Happy path | Accès au dashboard utilisateur |
| B1-P3 | Souscription à l'offre d'essai 1,90€ | Happy path + légal | Montant débité = 1,90€, aucun abonnement récurrent activé sans action explicite de l'utilisateur |
| B1-P4 | Déconnexion et accès sans session | Sécurité | Redirection vers login, aucun accès aux données |

> ⚠️ B1-P3 est le parcours le plus sensible du plan. Il doit être
> exécuté sur un environnement de test/staging avec un moyen de
> paiement sandbox. Si aucun environnement de ce type n'existe,
> le risque doit être explicitement accepté par le PO avant déploiement.


### B2 — Téléchargement PDF
**Score de risque : 9 | Priorité : P1**

Justification : point de conversion final, raison d'être du produit.
Dépend d'un moteur de rendu externe — composant fragile aux
variations de contenu et aux mises à jour de dépendances.

| ID | Parcours | Type | Critère de succès |
|---|---|---|---|
| B2-P1 | Téléchargement d'un CV complet (toutes rubriques remplies) | Happy path | Fichier PDF généré, lisible, mise en page conforme à la prévisualisation |
| B2-P2 | Téléchargement d'un CV partiel (rubriques minimales) | Cas limite | PDF généré sans erreur, sans zones vides anormales |
| B2-P3 | Téléchargement avec contenu long (textes dépassant la capacité d'une page) | Cas limite | Pagination correcte, aucun contenu tronqué |


### B3 — Création et édition du CV
**Score de risque : 6 | Priorité : P1**

Justification : cœur fonctionnel du produit. Une perte de données
ou un formulaire cassé détruit immédiatement la confiance utilisateur.

| ID | Parcours | Type | Critère de succès |
|---|---|---|---|
| B3-P1 | Création d'un CV de zéro (toutes rubriques) | Happy path | Toutes les rubriques sont saisissables, les données sont sauvegardées |
| B3-P2 | Modification d'un CV existant | Happy path | Les modifications sont persistées après rechargement de page |
| B3-P3 | Saisie de caractères spéciaux et accents (é, ü, ñ, @, &…) | Cas limite | Aucune corruption de données, affichage correct dans la prévisualisation |
| B3-P4 | Sauvegarde automatique (perte de connexion simulée) | Robustesse | Les données saisies avant la perte de connexion sont récupérées à la reconnexion |


### B4 — Assistance IA
**Score de risque : 6 | Priorité : P2**

Justification : fonctionnalité récente dépendant d'une API externe.
Sensible aux timeouts et aux réponses malformées. Non bloquante
mais différenciante — une régression serait visible immédiatement.

| ID | Parcours | Type | Critère de succès |
|---|---|---|---|
| B4-P1 | Génération d'une suggestion IA sur un champ rempli | Happy path | Suggestion affichée en moins de 10 secondes, contenu cohérent |
| B4-P2 | Génération d'une suggestion IA sur un champ vide | Cas limite | Message d'erreur explicite OU suggestion générique — pas de crash |
| B4-P3 | Comportement en cas de timeout API | Robustesse | Message d'erreur affiché, l'utilisateur peut continuer sans l'IA |


### B5 — Lettre de motivation
**Score de risque : 4 | Priorité : P2**

Justification : valeur ajoutée réelle mais non bloquante.
Partage l'infrastructure d'édition de B3 — une régression sur B3
impacte mécaniquement B5.

| ID | Parcours | Type | Critère de succès |
|---|---|---|---|
| B5-P1 | Création d'une lettre de motivation depuis un modèle | Happy path | Lettre générée, champs éditables, cohérente avec le CV associé |
| B5-P2 | Téléchargement de la lettre en PDF | Happy path | PDF généré, lisible, mise en page assortie au CV |


### B6 — Sélection de template
**Score de risque : 4 | Priorité : P2**

Justification : fonctionnalité de démarrage du parcours.
Un template non chargé bloque l'accès à l'éditeur.

| ID | Parcours | Type | Critère de succès |
|---|---|---|---|
| B6-P1 | Affichage et sélection d'un template | Happy path | Tous les templates s'affichent, la sélection lance l'éditeur |
| B6-P2 | Changement de template sur un CV existant | Happy path | Le contenu est conservé, le nouveau design est appliqué |


---

# 4. Stratégie d'environnement

## Principe méthodologique

En l'absence de données analytiques (Google Analytics, Mixpanel ou
équivalent), les choix d'environnement reposent sur trois sources :

1. **Les parts de marché globales 2025** (StatCounter GlobalStats)
   comme proxy de la base utilisateur probable.
2. **Le profil utilisateur cible** — un chercheur d'emploi qui crée
   son CV peut être sur n'importe quel device, mais la saisie longue
   favorise le desktop.
3. **La nature de l'application** — un éditeur de formulaire avec
   prévisualisation PDF est structurellement plus adapté au desktop.
   Le mobile est un risque de second ordre mais non négligeable.


## 1. Navigateurs

### Matrice de couverture

| Navigateur | Part de marché desktop (2025) | Priorité | Justification |
|---|:---:|:---:|---|
| Chrome (dernière version) | ~65% | 🔴 P1 | Navigateur majoritaire, moteur Blink de référence |
| Edge (dernière version) | ~13% | 🟠 P2 | Même moteur que Chrome (Blink) mais comportements CSS distincts |
| Safari (dernière version) | ~8% | 🔴 P1 | Moteur WebKit propre, rendu PDF et CSS souvent divergent |
| Firefox (dernière version) | ~7% | 🟠 P2 | Moteur Gecko indépendant, gestion des polices et du print différente |
| Chrome mobile | ~25% trafic mobile | 🟠 P2 | Représentatif du trafic mobile Android |
| Safari mobile (iOS) | ~18% trafic mobile | 🟠 P2 | Seul moteur autorisé sur iOS — ne pas confondre avec Safari desktop |

### Règle de couverture appliquée

On couvre **3 moteurs de rendu distincts** en priorité :
- **Blink** → Chrome (obligatoire), Edge (optionnel si temps limité)
- **WebKit** → Safari desktop + Safari iOS (un seul moteur, deux
  contextes d'usage très différents)
- **Gecko** → Firefox

> Tester uniquement Chrome et Edge revient à tester le même moteur deux fois 
> La divergence de rendu entre Blink, WebKit et Gecko sur la génération PDF et les formulaires complexes est documentée et constitue un risque réel 

### Ce qu'on ne teste pas (et pourquoi)

| Navigateur | Raison d'exclusion |
|---|---|
| Internet Explorer | Fin de support Microsoft en juin 2022 |
| Opera | Part de marché < 2%, moteur Blink identique à Chrome |
| Navigateurs anciens (N-2) | Effort disproportionné, utilisateurs marginaux |


## 2. Devices et systèmes d'exploitation

### Matrice de couverture

| Device | OS | Priorité | Justification |
|---|---|:---:|---|
| Desktop / Laptop | Windows 11 | 🔴 P1 | OS dominant sur le marché desktop (~70%) |
| Desktop / Laptop | macOS (dernière version) | 🔴 P1 | Nécessaire pour tester Safari dans ses conditions réelles |
| Smartphone | Android 13+ (ex: Samsung Galaxy S) | 🟠 P2 | Device Android de référence, représente la majorité du marché mobile |
| Smartphone | iOS 17+ (iPhone récent) | 🟠 P2 | Seul moyen de tester Safari iOS dans ses conditions réelles |
| Tablette | iPad (Safari iOS) | 🟡 P3 | Usage minoritaire, couvert partiellement par les tests iOS smartphone |

### Ce qu'on ne teste pas (et pourquoi)

| Device | Raison d'exclusion |
|---|---|
| Windows 10 | Comportement navigateur identique à W11, effort non justifié |
| Android < 10 | Part de marché marginale, devices en fin de vie |
| Linux desktop | Usage < 3%, profil développeur qui contourne les bugs |
| Tablette Android | Usage très minoritaire sur ce type d'application |


## 3. Résolutions d'écran

### Matrice de couverture

| Résolution | Catégorie | Priorité | Justification |
|---|---|:---:|---|
| 1920×1080 | Desktop Full HD | 🔴 P1 | Résolution desktop la plus répandue (~25% des sessions) |
| 1366×768 | Desktop HD | 🟠 P2 | Encore très présente sur les laptops d'entrée de gamme |
| 1280×800 | MacBook 13" | 🟠 P2 | Résolution de référence macOS laptop |
| 375×812 | iPhone (viewport standard) | 🟠 P2 | Référence mobile iOS |
| 360×800 | Android standard | 🟠 P2 | Référence mobile Android |
| 768×1024 | Tablette portrait | 🟡 P3 | Usage minoritaire, couvert si temps disponible |

## 4. Matrice de priorisation consolidée

Lecture : chaque cellule indique la priorité de test pour la combinaison navigateur × device 

|  | Windows 11 | macOS | Android | iOS |
|---|:---:|:---:|:---:|:---:|
| **Chrome** | 🔴 P1 | 🟠 P2 | 🟠 P2 | — |
| **Safari** | — | 🔴 P1 | — | 🟠 P2 |
| **Firefox** | 🟠 P2 | 🟡 P3 | — | — |
| **Edge** | 🟠 P2 | — | — | — |

### Combinaisons minimales pour le déploiement du jour

Si la contrainte de temps impose de réduire, ces 3 combinaisons constituent le filet de sécurité minimal non négociable :

1. **Chrome + Windows 11 + 1920×1080** — couvre la majorité des utilisateurs
2. **Safari + macOS + 1280×800** — seul moyen de détecter les bugs WebKit
3. **Chrome + Android + 375px** — valide la lisibilité mobile minimale

