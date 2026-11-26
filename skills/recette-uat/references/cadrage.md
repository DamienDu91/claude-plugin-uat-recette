# Cadrage de session (à chaque recette, pour chaque site)

Le cadrage est refait à chaque passage, même si le CDC contient déjà des paramètres : un site, une release et un environnement changent. Les valeurs du CDC servent de proposition à confirmer, jamais de réponse présumée. Objectif : travailler avec le contexte qu'aurait un expert UAT du site, pas seulement avec les lignes du CDC.

## Méthode

1. Lire le CDC d'abord, pour ne poser que les questions dont la réponse n'y figure pas ou qui doivent être reconfirmées.
2. Si Claude in Chrome est disponible, lister les onglets ouverts (`tabs_context_mcp`, lecture des seuls titres et URL) et proposer les correspondances probables. Ne pas lire le contenu d'un onglet non désigné.
3. Poser les questions en deux tours maximum, 4 questions par tour, avec des options concrètes et le champ libre. Ne pas poser de question dont la réponse est déjà établie.
4. Résumer le cadrage en 6 à 10 lignes et le faire confirmer avant le plan pré-vol. Écrire ce résumé en tête du PV.

## Tour 1 : onglets et cibles

- **Onglet front** : quel onglet Chrome (ou quelle URL) sert aux tests front. Proposer les onglets détectés.
- **Onglet back** : idem pour le back-office, avec le rôle de la session ouverte.
- **Environnement** de chaque onglet : staging, recette, préproduction, prod. Rapprocher l'hôte réel de la liste `hotes_staging`.
- **Autres URL utiles** : site de production de référence pour comparaison, maquettes, outil tiers à consulter (PIM, ERP, paiement test).

Règles :
- Un onglet désigné explicitement par l'utilisateur peut être réutilisé. Tout autre onglet reste intact.
- Désigner un onglet ne l'ajoute pas à la liste blanche des écritures : la porte staging se juge sur l'hôte réel et la liste `hotes_staging` du CDC.
- Si l'onglet désigné affiche une prod, le dire immédiatement : lecture seule.
- Si l'onglet n'est pas connecté, demander à l'utilisateur de se connecter lui-même.

## Tour 2 : nature du site et contexte métier

Poser ce qui manque parmi :

1. **Nature du site** : marchand B2C, marchand B2B / commande sur compte, catalogue sans vente en ligne, vitrine / corporate, contenu éditorial, application / outil métier, autre. Marchand ou non change l'essentiel de ce qui est critique.
2. **Objectif business** : quelle action le site doit produire (commande, demande de devis, génération de leads, information, prise de rendez-vous, authentification) et quels parcours sont vitaux.
3. **Nature du changement testé** : nouveau site, refonte, replatforming, nouvelle fonctionnalité, correctifs, montée de version. Distinguer non-régression et validation de nouveautés ; identifier ce qui a changé, le ticket ou la version.
4. **Audiences et rôles** : visiteurs, clients, clients B2B avec tarifs, commerciaux, administrateurs, contributeurs, rôles back à utiliser.
5. **Marchés** : langues, devises, pays, vues de magasin, régimes de taxes et d'expédition.
6. **Intégrations** : paiement, ERP, PIM, CRM, e-mail transactionnel, recherche, analytics, consentement. Savoir lesquelles sont réelles ou simulées en staging.
7. **Contraintes** : conformité (RGPD, cookies, mentions légales), accessibilité attendue, SEO à préserver, supports et navigateurs cibles, largeur mobile de référence.
8. **Risques connus** : bugs déjà identifiés, zones fragiles, cas à ne pas rejouer, données sensibles.
9. **Critères de gravité** de l'équipe : ce qu'on appelle bloquant pour ce site (par exemple, paiement impossible, tarif faux, perte de données).
10. **Données de test** : comptes et rôles disponibles (noms seulement), produits ou contenus de test, préfixe, cartes de test du prestataire de paiement si applicable.
11. **Échéance et enjeu de la recette** : mise en production prévue, jalon, seuil de décision.
12. **Temps manuel de référence** : combien de temps l'opérateur passerait à faire cette recette lui-même (total en minutes, ou minutes par cas). Estimation déclarative, notée comme hypothèse, utilisée pour le calcul du gain (voir `chronometrage.md`). Question toujours posée, jamais devinée ; sans réponse, le gain reste « non calculable ».

Ne pas poser les 11 d'un bloc : retenir les 4 à 8 plus structurantes selon le site et le CDC, cadrer le reste par défaut en le disant.

## Posture d'expert : ce que le contexte change

Utiliser le contexte pour exécuter les cas du CDC avec le regard d'un expert, sans les dépasser :

- **Site marchand** : porter une attention accrue à ce que les cas touchent déjà (prix, remises, taxes, frais de port, stock, devise, panier, checkout, e-mails de commande, statuts de commande, back-office commandes). Vérifier la cohérence entre le montant affiché, le montant calculé et celui du back.
- **Site marchand B2B** : tarifs par groupe client, minimums de commande, validations, comptes société.
- **Non marchand** : formulaires et leur acheminement, liens et navigation, cohérence du contenu, multilingue, médias, mentions et consentement, rendu mobile.
- **Application / outil** : droits par rôle, états d'erreur, persistance, reprise après rechargement.
- **Refonte / replatforming** : comparer à la référence indiquée (prod actuelle ou maquettes) sur les cas touchés, signaler les écarts de contenu ou de comportement.

Ce regard sert à mieux observer un cas du CDC. Il n'autorise pas d'ajouter des cas. Tout défaut manifeste vu en passant est noté en **observation hors CDC** (sans statut) dans la synthèse. Si l'utilisateur veut un balayage d'expert au-delà du CDC, le lui proposer comme option distincte, à approuver, et consigner ses constats dans une section séparée du PV.

## Consigner le cadrage

Dans le PV, section « Cadrage du passage » : onglets et URL utilisés, environnement, nature du site, objectif, nature du changement, rôles, marchés, intégrations, risques connus, critères de gravité fournis, hypothèses prises par défaut (marquées « hypothèse »). Aucun secret.

## Angles morts à nommer

- Les réponses de l'utilisateur sont des déclarations, pas des vérifications : confronter l'hôte réel, le rôle réel de la session et l'environnement réel.
- Un cadrage trop riche peut pousser à tester ce qui n'est pas dans le CDC. Rester dans le périmètre approuvé.
- Un site déclaré non marchand peut contenir un formulaire à effet réel (demande de devis, prise de rendez-vous) : demander ce qu'il déclenche avant toute écriture.
