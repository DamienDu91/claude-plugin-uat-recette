---
name: recette-rejeu
description: Rejoue après correctif les cas encore KO ou Bloqués d'une recette UAT (plus des cas sentinelles contre les régressions), sur le même cadrage, et écrit un nouveau passage lié au précédent avec le bilan consolidé (corrigés, persistants, régressions). À utiliser quand l'utilisateur dit « rejoue la recette », « rejeu après correctif », « les correctifs sont livrés », « revérifie les KO », « refais les cas en échec », « deuxième passage », ou fournit un cahier des charges déjà recetté en annonçant des corrections.
---

# Rejeu ciblé après correctif

Cycle réel d'une recette : constat, correctif par l'équipe de développement, rejeu, nouveau constat. Ce skill couvre le rejeu. Il suit toutes les règles de `recette-uat` (garde-fous, navigateur, consignation, chronométrage, bilan) et ne les redéfinit pas : lire d'abord `../recette-uat/SKILL.md`, puis `../recette-uat/references/garde-fous.md`, `navigateur.md`, `consignation.md`, `bilan.md` et `chronometrage.md` au moment utile.

Limite à garder en tête et à dire : un rejeu limité aux KO ne peut détecter aucune régression sur les cas qui étaient OK. Un correctif peut casser ce qui marchait. D'où les cas sentinelles ci-dessous.

## Déroulé

### 0. Chronométrage
Relever T0 dès le premier message et suivre `chronometrage.md`, comme pour une recette complète. Le rejeu a ses propres durées et son propre gain.

### 1. Lire l'état courant de la recette
- Lire le CDC, l'onglet « Résultats recette » et l'onglet « Consolidé » (voir `bilan.md`). Pour un Doc, lire les sections PV successives.
- Établir l'**état courant de chaque cas** = son dernier statut, tous passages confondus. Un KO du passage 1 non rejoué au passage 2 est toujours KO : partir de l'état courant, pas du seul dernier passage.
- Relever le dernier passage, son cadrage (section « Cadrage du passage ») et les décisions de l'opérateur.
- Aucun passage antérieur lisible : le dire et proposer `recette-uat`.

### 2. Cadrage allégé
Ne pas refaire tout le cadrage ; reprendre celui du passage précédent et faire confirmer ce qui a pu changer, en un seul tour de 4 questions maximum :
- **Correctifs livrés** : version, ticket ou liste de cas corrigés, date de déploiement, environnement. Sans correctif livré, le rejeu ne prouve rien : le dire.
- **Onglets et hôtes** : mêmes onglets front et back, même hôte, même environnement ? Re-détecter les onglets (`cadrage.md`) et confronter à la liste `hotes_staging`.
- **Changements de contexte** : nouveaux rôles, données de test, intégrations, risques connus.
- **Temps manuel de référence** du rejeu (estimation de l'opérateur ; peut être une fraction de la recette initiale).
L'approbation et les connexions du passage précédent ne se reportent pas : tout est redemandé.

### 3. Constituer le périmètre du rejeu
Cas retenus, par défaut :
1. **Cibles** : cas dont l'état courant est KO ou Bloqué.
2. **Sentinelles** : cas OK de priorité Haute (non-régression), plus les identifiants que l'utilisateur ajoute. C'est la partie qui détecte les régressions.

Exclusions, listées dans le plan avec leur raison :
- décision de l'opérateur « Rejeté (faux positif) » ou « Validé » : ne pas rejouer ;
- KO de décision « Correctif à planifier » dont le correctif n'est pas annoncé comme livré : proposer de les laisser hors rejeu (ils resteraient KO) ;
- Bloqué dont la cause n'est pas levée (droit, précondition, VPN, CAPTCHA, hôte hors liste blanche) : demander si elle l'est, sinon exclure ;
- Non testable : jamais rejoué tant que le CDC n'est pas corrigé ;
- Non exécuté : rejoué seulement si la raison (approbation, porte staging) a disparu.

Proposer trois portées et recommander la deuxième : (a) cibles seules, (b) cibles + sentinelles prioritaires, (c) recette complète (renvoyer alors vers `recette-uat`). Préciser le nombre de cas de chaque portée.

### 4. Plan pré-vol et approbation
Même plan que `recette-uat` (porte staging, liste exacte des cas écrivants, navigateur, lieu de consignation) avec en plus : passage précédent référencé, périmètre du rejeu, exclusions et leurs raisons. Attendre un oui explicite. Une approbation antérieure ne couvre pas ce passage.

### 5. Exécuter
- Nouvel identifiant de passage `PV-AAAA-MM-JJ-HHMM`, type **Rejeu de <PV précédent>**, noté dans « Synthèses recette ».
- Mêmes règles d'exécution et de statuts que `recette-uat` (étapes exactes, une relance avant KO, captures, consignation au fil de l'eau).
- Dans « Obtenu / écart », commencer par `Rejeu de <PV précédent> :` ; pour une sentinelle : `Sentinelle (non-régression) :`.
- Pour un cas toujours KO, dire si l'écart est **identique** ou **différent** de celui du passage précédent, en citant les deux valeurs.
- Ne pas réutiliser ni supprimer les données de test du passage précédent : en créer de nouvelles avec le préfixe, et les lister.
- Un cas sentinelle passé de OK à KO est une **régression** : le signaler immédiatement à l'utilisateur, sans attendre la clôture.

### 6. Clôture
1. Mettre à jour « Consolidé » et « Bilan recette » (voir `bilan.md`, vue consolidée et évolution) : **Corrigés**, **Persistants**, **Régressions**, **Stables OK**, avec la liste nominative des trois premiers.
2. Laisser « Décision opérateur » vide pour les lignes du rejeu.
3. Écrire le chronométrage et le gain du rejeu, et le cumul des passages si la référence manuelle le permet.
4. Réponse en peu de mots : corrigés, persistants, régressions (identifiants + écart en une ligne), état consolidé (OK/KO/Bloqué), couverture, durée active Claude, ce qui exige une décision humaine. Ajouter : « Les cas non rejoués gardent leur dernier statut ; leur conformité après correctifs n'est pas vérifiée. »

## Angles morts à nommer
- Un cas Corrigé est OK sur les étapes du CDC, pas garanti sans effet de bord ailleurs : seules les sentinelles couvrent ce risque, et partiellement.
- Un OK au rejeu sur un cas Bloqué avant n'est pas équivalent à un OK initial si l'environnement a changé entre-temps : noter les différences de cadrage.
- Rejouer un faux positif du testeur le reproduit : si l'opérateur a rejeté le KO, ne pas le rejouer sans raison.
- Le gain de temps d'un rejeu est plus faible que celui d'une recette complète, mais son intérêt tient à la fréquence : ne pas le juger sur un passage isolé.
