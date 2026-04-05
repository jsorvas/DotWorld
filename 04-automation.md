# Approche d'automatisation 


>1. **Choix du framework** : 

Cypress ou Playwright

Fortes communautés, et 2 outils qui s'intègrent bien avec des Tests web + API



>2. **Architecture de test** : Comment structurerais-tu ton projet de tests automatisés ? (arborescence, page objects, fixtures…)

Commencer par des tests critiques (P1) couvrant 80% des usages les plus fréquents (loi de Pareto)

Ecrire les tests automatisés en Page Object Model afin de pouvoir le maintenir facilement.



>3. **Scénario prioritaire** : Écris le script réel pour automatiser **1 parcours critique** identifié en Partie 2

Cypress requiert un environnement Node.js

### Structure du Repository

```text
makemycv-cypress-tests/
├── .gitignore
├── package.json
├── cypress.config.js
├── README.md
└── cypress/
    ├── e2e/
    │   └── registration/
    │       └── TC-001_inscription_email_valide.cy.js
    ├── fixtures/
    │   └── users.json
    └── support/
        ├── commands.js
        └── e2e.js
```

### 1. `package.json`
Ce fichier permet d'installer les dépendances (Cypress) et définit les scripts de lancement.

```json
{
  "name": "makemycv-cypress-tests",
  "version": "1.0.0",
  "description": "Tests automatisés E2E pour MakeMyCV",
  "main": "index.js",
  "scripts": {
    "test:open": "cypress open",
    "test:run": "cypress run --browser chrome"
  },
  "devDependencies": {
    "cypress": "^13.0.0"
  }
}
```

---

### 2. `cypress.config.js`
Le fichier de configuration de Cypress, résolution requise (1920×1080) et URL de base.

```javascript
const { defineConfig } = require("cypress");

module.exports = defineConfig({
  e2e: {
    baseUrl: 'https://makemycv.com',
    viewportWidth: 1920,
    viewportHeight: 1080,
    setupNodeEvents(on, config) {
      // Implémentation des écouteurs d'événements Node ici si nécessaire
    },
  },
});
```

---

### 3. `cypress/e2e/registration/TC-001_inscription_email_valide.cy.js`
Script de test automatisé 

*Note : Les sélecteurs CSS (`[data-cy="..."]` ou `input[name="..."]`) sont hypothétiques et devront être adaptés au code source réel de `makemycv.com`.*

```javascript
describe('TC-001 — Inscription avec un email valide', () => {
  
  // Génération d'un email unique pour éviter le blocage "Compte existant" (Précondition 1)
  const uniqueId = Date.now();
  const testEmail = `test.makemycv+${uniqueId}@example.com`;
  const testPassword = 'Password123!Maj'; // Respect de la complexité standard

  beforeEach(() => {
    // Cypress gère automatiquement le nettoyage du cache et des cookies entre les tests.
    // La résolution 1920x1080 est déjà définie dans cypress.config.js
    
    // Interception de la requête d'inscription pour vérifier qu'elle s'exécute sans erreur
    cy.intercept('POST', '**/api/auth/register').as('registerRequest');
  });

  it('Inscrit un nouvel utilisateur et le redirige vers le dashboard', () => {
    // Étape 1 : Accéder à https://makemycv.com
    cy.visit('/');

    // Étape 2 : Cliquer sur le bouton "S'inscrire"
    // Bonne pratique : utiliser des attributs data-cy dans le code source
    cy.get('[data-cy="btn-signup"]').click();

    // Vérification que nous sommes bien sur la page d'inscription
    cy.url().should('include', '/signup');

    // Étape 3 : Saisir un email valide au format standard
    cy.get('input[name="email"]')
      .should('be.visible')
      .type(testEmail);

    // Étape 4 : Saisir un mot de passe respectant les règles
    cy.get('input[name="password"]')
      .type(testPassword);

    // Étape 5 : Confirmer le mot de passe
    cy.get('input[name="confirmPassword"]')
      .type(testPassword);

    // Étape 6 : Valider le formulaire
    cy.get('[data-cy="btn-submit-registration"]').click();

    // RÉSULTATS ATTENDUS

    // -> Le compte est créé sans erreur (Code HTTP 200 ou 201 attendu sur l'API)
    cy.wait('@registerRequest').its('response.statusCode').should('be.oneOf', [200, 201]);

    // -> L'utilisateur est redirigé vers son dashboard
    cy.url().should('include', '/dashboard');
    cy.get('[data-cy="dashboard-header"]').should('contain', 'Bienvenue');

    // -> Aucun abonnement payant n'est activé à cette étape
    // Vérification via l'interface utilisateur ou l'état du compte affiché
    cy.get('[data-cy="plan-status"]').should('contain.text', 'Gratuit');
    
  });
});
```

---

### 4. `README.md`

```markdown
# MakeMyCV - Cypress E2E Tests

Ce repository contient les tests automatisés End-to-End (E2E) pour l'application MakeMyCV, écrits avec Cypress.

## Prérequis

- [Node.js](https://nodejs.org/) (version 14 ou supérieure recommandée)
- Google Chrome installé sur votre machine

## Installation

1. Clonez ce repository :
   ```bash
   git clone [https://github.com/votre-organisation/makemycv-cypress-tests.git](https://github.com/votre-organisation/makemycv-cypress-tests.git)
   ```
2. Accédez au dossier :
   ```bash
   cd makemycv-cypress-tests
   ```
3. Installez les dépendances :
   ```bash
   npm install
   ```

## Exécution des tests

**Exécuter les tests en mode "Headless" (en arrière-plan, idéal pour la CI/CD) :**
```bash
npm run test:run
```

**Ouvrir l'interface graphique de Cypress (Mode interactif) :**
```bash
npm run test:open
```

## Couverture de test

- **[TC-001]** : Inscription d'un nouvel utilisateur avec un email valide (Sévérité : Critique)

---

>4. **Stratégie CI/CD** : Comment intégrerais-tu ces tests dans une pipeline CI/CD ?

Pour que cette intégration soit robuste, voici trois règles d'or à respecter :

Ne testez pas directement en production : L'URL de base (baseUrl) dans votre cypress.config.js devrait pointer vers un environnement de Recette ou de Pré-production. La CI/CD doit valider le code avant qu'il ne touche les vrais utilisateurs.

Gérez vos données de test : Dans notre script précédant, création email unique. C'est une bonne astuce, mais à la longue, cela pollue la base de données. L'idéal en CI/CD est d'avoir une tâche (souvent via une requête API cachée) qui nettoie la base de données de test avant ou après chaque exécution.

Sécurisez vos identifiants : Ne laissez jamais de vrais mots de passe ou des clés d'API (comme celle d'un service d'emails comme MailSlurp) en clair dans le code.

Utiliser un outil d'intégration continue (comme GitLab CI, Jenkins ou Bitbucket Pipelines).

