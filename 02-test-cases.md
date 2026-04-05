# 3 Cas de test — F1 Gestion compte & abonnement

> Parcours nominal (happy path) | Score de risque : 9 | Priorité : P1

## TC-001 — Inscription avec un email valide

| Champ | Description |
|---|---|
| **ID** | TC-001 |
| **Titre** | Inscription d'un nouvel utilisateur avec un email valide |
| **Préconditions** | - Aucun compte existant associé à l'email utilisé |
| | - Navigateur : Chrome dernière version, résolution 1920×1080 |
| | - Connexion réseau stable |
| **Étapes** | 1. Accéder à https://makemycv.com |
| | 2. Cliquer sur le bouton "S'inscrire" ou équivalent |
| | 3. Saisir un email valide au format standard (ex : test@example.com) |
| | 4. Saisir un mot de passe respectant les règles de complexité affichées |
| | 5. Confirmer le mot de passe |
| | 6. Valider le formulaire en cliquant sur "Créer mon compte" ou équivalent |
| **Résultat attendu** | - Le compte est créé sans erreur |
| | - L'utilisateur est redirigé vers son dashboard ou une page de confirmation |
| | - Un email de confirmation est reçu dans la boîte de l'adresse utilisée |
| | - Aucun abonnement payant n'est activé à cette étape |
| **Résultat obtenu** | *(à renseigner lors de l'exécution)* |
| **Statut** | ⬜ Non exécuté |
| **Sévérité** | Critique |


## TC-002 — Connexion avec des identifiants valides

| Champ | Description |
|---|---|
| **ID** | TC-002 |
| **Titre** | Connexion d'un utilisateur existant avec email et mot de passe valides |
| **Préconditions** | - Un compte actif existe pour l'email utilisé (TC-001 passé avec succès) |
| | - L'utilisateur est déconnecté |
| | - Navigateur : Chrome dernière version, résolution 1920×1080 |
| **Étapes** | 1. Accéder à https://makemycv.com |
| | 2. Cliquer sur "Se connecter" ou équivalent |
| | 3. Saisir l'email du compte existant |
| | 4. Saisir le mot de passe associé |
| | 5. Cliquer sur "Connexion" ou équivalent |
| **Résultat attendu** | - L'utilisateur est authentifié sans erreur |
| | - Il est redirigé vers son dashboard personnel |
| | - Ses données (CV existants, paramètres) sont bien présentes |
| | - Aucune action de paiement n'est déclenchée |
| **Résultat obtenu** | *(à renseigner lors de l'exécution)* |
| **Statut** | ⬜ Non exécuté |
| **Sévérité** | Critique |


## TC-003 — Souscription à l'offre d'essai (1,90€ / 14 jours)

| Champ | Description |
|---|---|
| **ID** | TC-003 |
| **Titre** | Souscription à l'offre d'essai sans activation d'abonnement récurrent non consenti |
| **Préconditions** | - Utilisateur connecté (TC-002 passé avec succès) |
| | - Environnement de paiement sandbox disponible |
| | - Carte de test fournie par le prestataire de paiement |
| | - Navigateur : Chrome dernière version, résolution 1920×1080 |
| **Étapes** | 1. Depuis le dashboard, déclencher une action nécessitant l'offre payante |
| | (ex : téléchargement PDF, accès template premium) |
| | 2. Cliquer sur "Essayer pour 1,90€" ou équivalent |
| | 3. Vérifier que les conditions de l'offre sont affichées clairement avant paiement |
| | (montant, durée, conditions de renouvellement) |
| | 4. Saisir les coordonnées de la carte de test sandbox |
| | 5. Valider le paiement |
| | 6. Vérifier le récapitulatif de commande affiché post-paiement |
| | 7. Vérifier l'email de confirmation reçu |
| | 8. Accéder aux paramètres du compte > section abonnement |
| **Résultat attendu** | - Le montant débité est exactement 1,90€, pas davantage |
| | - Les conditions de renouvellement à 27,95€/mois sont affichées |
| | explicitement AVANT la validation du paiement |
| | - L'email de confirmation mentionne la date de fin d'essai |
| | et le montant du renouvellement automatique |
| | - La section abonnement affiche clairement la date de renouvellement |
| | et propose un lien de résiliation accessible |
| **Résultat obtenu** | *(à renseigner lors de l'exécution)* |
| **Statut** | ⬜ Non exécuté |
| **Sévérité** | Critique |



2 
