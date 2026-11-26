---
name: recette-init
description: Prépare ou contrôle le cahier des charges d'un site avant une recette UAT (onglet Paramètres de recette, cas de test vérifiables, porte staging). À utiliser quand l'utilisateur dit « prépare la recette », « initialise la recette de ce site », « vérifie mon cahier des charges », « mon CDC est-il testable », « crée l'onglet paramètres de recette », ou avant un premier passage sur un nouveau site.
---

# Préparation et contrôle d'un CDC de recette

Objectif : rendre un CDC exécutable par le skill `recette-uat` et dire honnêtement ce qui ne l'est pas. Aucune exécution sur le site dans ce skill : lecture du CDC uniquement, sauf écriture demandée de l'onglet de paramètres.

## Déroulé

1. Obtenir le lien du CDC (Google Doc ou Sheet). Le lire (outils Google Docs ou Sheets).
2. Lire `../recette-uat/references/gabarit-cdc.md` pour la structure attendue.
3. Contrôler et produire un rapport court :
   - **Paramètres** : lesquels existent, lesquels manquent. Signaler en priorité l'absence de `hotes_staging` (aucune écriture possible), de `roles_comptes`, de `prefixe_donnees_test`, de `emails_effets_de_bord`.
   - **Cas** : nombre total, répartition front/back, nombre de cas écrivants identifiés, nombre de cas sans résultat attendu vérifiable (futurs **Non testable**), doublons d'identifiants, cas sans identifiant.
   - **Cas faibles** : citer les 5 plus problématiques avec une reformulation proposée du résultat attendu. Ne pas réécrire dans le CDC sans demande.
   - **Risques de recette** : dépendances tierces (paiement, ERP, e-mail), CAPTCHA connu, accès VPN/SSO, rôles manquants.
4. Proposer, sans l'appliquer, la création de l'onglet ou de la section « Paramètres de recette » pré-remplie avec ce qui est déductible du CDC. L'écrire seulement après accord, en ajout uniquement.
5. Ne jamais demander ni écrire de mot de passe, clé ou secret. Les rôles sont des noms, pas des identifiants.
6. Terminer par un verdict en une ligne : « prêt pour recette », « prêt avec réserves (liste) » ou « pas prêt (raison) ».

## Angles morts à signaler

- Un CDC très complet sur le front et mince sur le back (ou l'inverse) fausse la couverture : le dire.
- Un CDC rédigé après la réalisation tend à décrire l'existant : les cas passeront, ce qui ne prouve pas la conformité au besoin. Le dire si les cas ressemblent à une description d'écran.
