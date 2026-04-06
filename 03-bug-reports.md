# Rapport de bug — BUG-001

| Champ | Description |
|---|---|
| **ID** | BUG-001 |
| **Titre** | Formulaire d'inscription : aucun message d'erreur affiché pour un mot de passe sans chiffre |
| **Sévérité** | Majeur |
| **Étapes de reproduction** | 1. Accéder à https://makemycv.com |
| | 2. Cliquer sur "S'inscrire" |
| | 3. Saisir un email valide non utilisé : nouveau@example.com |
| | 4. Saisir un mot de passe sans chiffre : "Motdepasse!" |
| | 5. Cliquer sur "Créer mon compte" |
| **Résultat attendu** | La soumission est bloquée et un message d'erreur explicite est affiché sous le champ mot de passe : |
| | "Le mot de passe doit contenir au moins un chiffre" ou équivalent |
| **Résultat obtenu** | Le formulaire est soumis sans erreur malgré l'absence de chiffre dans le mot de passe |
| | Le compte est créé avec un mot de passe ne respectant pas les règles de complexité affichées dans l'interface |
| **Environnement** | Navigateur : Chrome 124, Windows 11, 1920×1080 |
| **Capture d'écran** | *(à joindre : screenshot du formulaire soumis avec succès malgré le mot de passe invalide "Motdepasse!")* |

---

# Rapport de bug — BUG-002

| Champ | Description |
|---|---|
| **ID** | BUG-002 |
| **Titre** | Résiliation abonnement : aucun lien ou bouton de résiliation accessible depuis les paramètres du compte |
| **Sévérité** | Critique |
| **Étapes de reproduction** | 1. Se connecter avec un compte ayant un abonnement d'essai actif à 1,90€ |
| | 2. Depuis le dashboard, accéder aux paramètres du compte |
| | 3. Naviguer jusqu'à la section "Abonnement" ou équivalent |
| | 4. Chercher un bouton ou lien "Résilier mon abonnement" ou équivalent |
| | 5. Aucun élément de résiliation n'est trouvé dans l'interface |
| **Résultat attendu** | Un bouton ou lien "Résilier mon abonnement" est visible et accessible en moins de 3 clics depuis le dashboard |
| **Résultat obtenu** | La section "Abonnement" affiche le statut et la date de renouvellement mais ne propose aucune option de résiliation |
| | Aucun lien, bouton ou mention d'une procédure de résiliation n'est présent dans l'interface |
| **Environnement** | Navigateur : Chrome 124, Windows 11, 1920×1080 |
| **Capture d'écran** | *(à joindre : screenshot de la section "Abonnement" sans option de résiliation visible)* |
