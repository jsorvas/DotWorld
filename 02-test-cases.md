# 3 Cas de test (happy path) — F1 Gestion compte & abonnement

> Parcours nominal | Score de risque : 9 | Priorité : P1

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



# 2 Cas de test (edge cases) — F1 Gestion compte & abonnement

> Cas limites | Score de risque : 9 | Priorité : P1

---

## TC-004 — Inscription avec un email déjà utilisé

| Champ | Description |
|---|---|
| **ID** | TC-005 |
| **Titre** | Tentative d'inscription avec un email déjà associé à un compte existant |
| **Préconditions** | - Un compte actif existe pour l'email test@example.com |
| | - L'utilisateur est déconnecté |
| | - Navigateur : Chrome dernière version, résolution 1920×1080 |
| **Étapes** | 1. Accéder à https://makemycv.com |
| | 2. Cliquer sur "S'inscrire" ou équivalent |
| | 3. Saisir l'email d'un compte déjà existant (test@example.com) |
| | 4. Saisir un mot de passe valide |
| | 5. Confirmer le mot de passe |
| | 6. Valider le formulaire |
| **Résultat attendu** | - L'inscription est refusée |
| | - Un message d'erreur explicite est affiché : |
| | "Un compte existe déjà avec cet email" ou équivalent |
| | - Le message d'erreur ne révèle pas d'informations sensibles |
| | sur le compte existant (ex : prénom, statut abonnement) |
| | - Aucun doublon de compte n'est créé en base |
| | - Aucun email parasite n'est envoyé au propriétaire du compte existant |
| **Résultat obtenu** | *(à renseigner lors de l'exécution)* |
| **Statut** | ⬜ Non exécuté |
| **Sévérité** | Majeur |

> ⚠️ **Note de sécurité** : si l'application confirme l'existence
> d'un compte via le message d'erreur, elle expose une surface
> d'énumération d'emails (user enumeration). Ce comportement,
> même s'il améliore l'UX, constitue une vulnérabilité OWASP A07
> à signaler au PO pour arbitrage conscient.

---

## TC-005 — Résiliation de l'abonnement pendant la période d'essai

| Champ | Description |
|---|---|
| **ID** | TC-006 |
| **Titre** | Résiliation de l'abonnement avant la fin de la période d'essai de 14 jours |
| **Préconditions** | - Utilisateur connecté avec un abonnement d'essai actif à 1,90€ |
| | (TC-003 passé avec succès) |
| | - La période d'essai de 14 jours n'est pas expirée |
| | - Environnement de paiement sandbox disponible |
| | - Navigateur : Chrome dernière version, résolution 1920×1080 |
| **Étapes** | 1. Depuis le dashboard, accéder aux paramètres du compte |
| | 2. Naviguer jusqu'à la section "Abonnement" ou équivalent |
| | 3. Vérifier que l'option de résiliation est visible et accessible |
| | sans obstacle (pas de dark pattern — ex : bouton caché, |
| | tunnel de rétention abusif) |
| | 4. Cliquer sur "Résilier mon abonnement" ou équivalent |
| | 5. Confirmer la résiliation si une étape de confirmation est proposée |
| | 6. Vérifier l'email de confirmation de résiliation reçu |
| | 7. Vérifier le statut de l'abonnement dans les paramètres du compte |
| | 8. Attendre la date de renouvellement théorique et vérifier |
| | qu'aucun prélèvement supplémentaire n'est effectué |
| **Résultat attendu** | - La résiliation est possible en moins de 3 clics depuis le dashboard |
| | - Un email de confirmation de résiliation est reçu immédiatement |
| | - Le statut abonnement indique "Résilié" ou "Actif jusqu'au JJ/MM/AAAA" |
| | - Aucun prélèvement à 27,95€ n'est effectué après la résiliation |
| | - L'accès aux fonctionnalités premium est maintenu |
| | jusqu'à la fin de la période d'essai payée |
| **Résultat obtenu** | *(à renseigner lors de l'exécution)* |
| **Statut** | ⬜ Non exécuté |
| **Sévérité** | Critique |

> ⚠️ **Note légale** : en droit européen (directive Omnibus 2022
> et Code de la consommation), la résiliation d'un abonnement en
> ligne doit être aussi simple que la souscription. Un obstacle
> à la résiliation est une infraction sanctionnable par la DGCCRF.
> Tout friction anormale observée lors de ce test doit être remontée
> immédiatement comme défaut bloquant, indépendamment du go/no-go
> technique.
