# Consignation des résultats dans le CDC

Règle d'or : **ajout seulement**. Ne jamais modifier ni supprimer le texte des exigences, des cas ou des passages précédents. Chaque passage de recette ajoute un bloc daté. L'historique Google (révisions) sert de filet, mais ne pas s'y fier : n'écraser rien.

## Identifiant de passage

`PV-AAAA-MM-JJ-HHMM` (heure de Paris). Réutilisé dans le bloc de résultats, les noms de captures et la synthèse.

## CDC au format Google Sheet

1. Lire la liste des onglets. Chercher l'onglet « Résultats recette ». S'il n'existe pas, le créer (ajout d'un onglet, aucune modification des autres).
2. Colonnes de l'onglet « Résultats recette » (créer l'en-tête s'il est vide) :

| Passage | ID cas | Périmètre | Priorité | Statut | Attendu | Obtenu / écart | URL | Navigateur | Capture | Horodatage | Décision opérateur | Commentaire opérateur |
|---|---|---|---|---|---|---|---|---|---|---|---|---|

Ajouter en fin de tableau la colonne **N : Durée cas (s)** (voir `chronometrage.md`), sans déplacer les colonnes A à M.

L'onglet « Bilan recette » (voir `bilan.md`) lit ces colonnes : respecter leur ordre et leurs libellés de statut exacts.

3. Une ligne par cas et par passage, ajoutée à la suite (jamais de remplacement d'une ligne existante).
4. « Décision opérateur » et « Commentaire opérateur » restent vides : valeurs attendues `Validé`, `Correctif à planifier`, `Rejeté (faux positif)`, `À rejouer`.
5. Une ligne de synthèse par passage dans un onglet « Synthèses recette » (ou en tête du bloc si l'onglet n'existe pas) : passage, date, environnement, hôte, navigateur et motif de bascule éventuel, compteurs OK / KO / Bloqué / Non testable / Non exécuté, données de test créées.
5bis. Créer ou mettre à jour l'onglet « Bilan recette » en première position, selon `bilan.md`. Il fait partie du livrable : une recette sans bilan n'est pas terminée.
6. Écrire au fil de l'eau (une ligne par cas terminé), pas en bloc final.
7. Pour les valeurs avec formules ou mise en forme du CDC existant, ne pas toucher aux cellules d'origine.

## CDC au format Google Doc

1. Aller à la fin du document. Ajouter un saut de page puis une section « Procès-verbal de recette PV-… ».
2. Contenu de la section : en-tête (date, environnement, hôte, navigateur, motif de bascule), puis un tableau à colonnes `ID cas | Statut | Attendu | Obtenu / écart | Capture | Décision opérateur`, puis la synthèse chiffrée et la liste des données de test créées.
3. Ajouter chaque ligne de cas à la suite au fil de l'eau. Ne pas réécrire le reste du document.
3bis. Sous l'en-tête, inclure la section « Cadrage du passage » (onglets et URL, environnement, nature et objectif du site, changement testé, rôles, marchés, intégrations, risques connus, hypothèses). Pour un Sheet : même contenu dans l'onglet « Synthèses recette », colonne ou bloc « Cadrage ».
4. Si le Doc contient des cas dans un tableau d'origine, ne pas y insérer de colonne : tout va dans la section PV.
5. Si les outils d'édition du Doc ne permettent pas d'ajouter un tableau, utiliser une liste structurée une ligne par cas avec les mêmes champs, et le dire dans la réponse.

## Captures d'écran

Les captures sont l'élément le plus fragile : les connecteurs Google ne garantissent pas l'insertion d'images dans une cellule ou un Doc.

1. Nommer chaque capture `<PV>_<ID-CAS>_<statut>.png`.
2. Si un outil Drive permet de téléverser le fichier dans un dossier, téléverser dans un dossier « Recette <site> / <PV> » (le créer si l'utilisateur l'a autorisé ou s'il est indiqué dans les paramètres) et écrire le lien dans la colonne « Capture ».
3. Sinon, inscrire le **nom de fichier** dans la colonne « Capture » et, à la clôture, remettre toutes les captures à l'utilisateur en un seul fichier compressé via l'envoi de fichiers. Le dire clairement dans la réponse finale : « captures remises séparément, référencées par nom ».
4. Ne jamais prétendre qu'une capture est liée ou insérée sans l'avoir vérifié.
5. Si une capture montre des données personnelles réelles, l'indiquer dans « Obtenu / écart » (voir garde-fous).

## Vérification d'écriture

Après la première écriture du passage, relire la zone écrite et confirmer que la ligne est présente et lisible. Si l'écriture échoue : le dire, conserver les résultats dans la conversation, ne pas continuer à déclarer qu'ils sont consignés.

## Rejeu d'un passage

Un rejeu crée un nouveau passage (nouveaux identifiant et lignes), suivant le skill `recette-rejeu`. Dans la colonne « Obtenu / écart », référencer le passage précédent (« Rejeu de PV-… : ») ; pour un cas sentinelle, « Sentinelle (non-régression) : ». Dans « Synthèses recette », renseigner le type de passage (Recette initiale / Rejeu de PV-…) et le périmètre rejoué. Créer ou mettre à jour l'onglet « Consolidé » décrit dans `bilan.md`.
