# Onglet « Bilan recette » (lecture en un coup d'œil)

But : permettre à l'opérateur humain de juger l'état d'une recette sans lire les lignes. Le bilan décrit, il ne décide pas. La décision de mise en production reste humaine.

## Sheet : onglet « Bilan recette »

Créer l'onglet en **première position** (ajout, sans toucher aux autres onglets). Le remplir avec des **formules** qui lisent « Résultats recette », pour que les compteurs se mettent à jour quand l'opérateur renseigne ses décisions. Colonnes de « Résultats recette » : A Passage, B ID cas, C Périmètre, D Priorité, E Statut, F Attendu, G Obtenu / écart, H URL, I Navigateur, J Capture, K Horodatage, L Décision opérateur, M Commentaire opérateur.

### Bloc 1 : en-tête
- Site, environnement, hôte, date, version ou ticket testé (valeurs écrites par le plugin).
- `Passage affiché` : dernier passage, `=INDEX('Résultats recette'!A:A,COUNTA('Résultats recette'!A:A))`. L'opérateur peut y saisir un autre identifiant pour consulter un ancien passage.
- `Passage précédent` : identifiant écrit par le plugin à la création du passage (valeur, pas formule), ou « — » au premier passage.

### Bloc 2 : statuts (le cœur du bilan)
Une ligne par statut : OK, KO, Bloqué, Non testable, Non exécuté, puis Total.
- Nombre : `=COUNTIFS('Résultats recette'!$A:$A,$B$3,'Résultats recette'!$E:$E,A7)`
- Total : `=COUNTIF('Résultats recette'!$A:$A,$B$3)`
- Part : nombre ÷ total.
- Barre : `=SPARKLINE(B7,{"charttype","bar";"max",$B$13})` (visuel dans la cellule, sans objet graphique).
- Passage précédent et écart (Δ) pour chaque statut, avec les mêmes formules sur `$B$4`.
Colorer les statuts : OK vert, KO rouge, Bloqué orange, Non testable et Non exécuté gris, via la mise en forme conditionnelle si l'outil le permet. Sinon, ne pas insister.

### Bloc 3 : indicateurs de lecture
- **Couverture exécutée** = (OK + KO) ÷ Total. Elle dit quelle part du CDC a réellement été jugée.
- **Réussite sur cas exécutés** = OK ÷ (OK + KO).
- **À revoir par l'humain** = KO et Bloqué dont « Décision opérateur » est vide.
- **Décisions prises** : Validé, Correctif à planifier, Rejeté (faux positif), À rejouer, comptées sur la colonne L.
- **Régressions** : cas OK au passage précédent devenus KO, listés par le plugin (liste écrite, pas formule), vide au premier passage.

### Bloc 4 : ventilations
- Statuts × Périmètre (Front, Back).
- Statuts × Priorité (Haute, Moyenne, Basse), avec la correspondance de libellés du CDC écrite sous le tableau.
Formules : `COUNTIFS` sur A, E et C ou D.

### Bloc 5 : indicateur de qualité (seulement si les seuils sont définis)
Les seuils viennent des paramètres du CDC : `seuil_ko_prioritaire_max`, `seuil_reussite_min`, `seuil_couverture_min`. Les copier dans le bloc en valeurs visibles.
- Un seuil manquant : afficher « Seuils non définis : pas d'indicateur » et rien d'autre. Ne jamais inventer un seuil.
- Seuils définis : trois états lisibles, **Prêt à valider** (tous les seuils respectés), **Réserves** (au moins un seuil manqué hors KO prioritaires), **Non prêt** (KO prioritaires au-dessus du maximum).
- Toujours afficher sous l'indicateur : « Indicateur calculé, non une décision. Validation humaine requise. »

### Onglet « Consolidé » et bloc 7 : état courant et évolution (indispensable dès le premier rejeu)
Les blocs précédents décrivent le **passage affiché**. Après un rejeu, qui ne contient que les cas rejoués, la qualité réelle se lit sur l'**état courant** : le dernier statut de chaque cas, tous passages confondus.

Onglet « Consolidé » (ajout, après « Bilan recette »), une ligne par identifiant de cas, valeurs écrites par le plugin pour la colonne A, formules pour les autres, remplies pour toutes les lignes (la plage `$2:$` désigne les lignes de « Résultats recette », en ajout seulement, donc le dernier match est le plus récent) :
- A : ID cas (valeur).
- B : statut courant `=LOOKUP(2,1/('Résultats recette'!$B$2:$B=$A2),'Résultats recette'!$E$2:$E)`.
- C : passage du dernier statut, même formule sur la colonne A de « Résultats recette ».
- D : statut précédent `=IFERROR(INDEX(FILTER('Résultats recette'!$E$2:$E,'Résultats recette'!$B$2:$B=$A2),COUNTIF('Résultats recette'!$B$2:$B,$A2)-1),"")`.
- E : évolution : « Nouveau » si D vide ; « Corrigé » si D ∈ {KO, Bloqué} et B = OK ; « Persistant » si D ∈ {KO, Bloqué} et B ∈ {KO, Bloqué} ; « Régression » si D = OK et B = KO ; « Stable OK » si D = OK et B = OK ; sinon « Autre ».
- F : périmètre et G : priorité (`INDEX/MATCH` sur « Résultats recette »).

Bloc 7 du Bilan :
- **État consolidé** : nombre de OK, KO, Bloqué, Non testable, Non exécuté sur la colonne B, part, couverture exécutée consolidée. C'est le chiffre à regarder pour juger l'état de la recette.
- **Évolution** : nombre de Corrigés, Persistants, Régressions, Stables OK, Nouveaux (`COUNTIF` sur la colonne E).
- **Listes nominatives** des Corrigés, Persistants et Régressions (`FILTER` sur « Consolidé », ou valeurs écrites par le plugin si `FILTER` échoue).
Afficher clairement les deux vues : « Passage PV-… » et « État consolidé de la recette ». Pour un Doc, écrire un tableau consolidé figé dans la section du rejeu.
La seule évolution connue est celle entre les deux derniers passages d'un cas : les cas non rejoués gardent leur statut.

### Bloc 6 : temps et gain (voir `chronometrage.md`)
Valeurs écrites par le plugin pour le passage : durée totale écoulée, attente humaine (avec le détail des motifs sur une ligne), durée active Claude. Formules : durée moyenne par cas `=AVERAGEIFS('Résultats recette'!$N:$N,'Résultats recette'!$A:$A,$B$3)` et somme des durées de cas `=SUMIFS(...)`.
Cellules de saisie pour l'opérateur, bien visibles : **temps manuel de référence** (minutes, pré-rempli avec la valeur donnée au cadrage, marqué « estimation ») et **temps de revue opérateur** (minutes, vide).
Calcul : **temps humain mobilisé** = attente humaine + temps de revue ; **gain estimé** = référence − temps humain mobilisé. Si la référence manque : « gain non calculable » (afficher la durée Claude seule). Écrire sous le bloc : « La durée Claude mesure le délai d'exécution, pas le temps humain économisé. Gain = estimation à partir de la référence saisie. »

### Contrôle après écriture
Relire le bilan : aucune cellule en erreur, et Total = nombre de lignes du passage dans « Résultats recette ». Une formule en erreur (syntaxe, séparateur selon la langue du classeur) se corrige avant de présenter le bilan ; si l'erreur persiste, écrire les valeurs calculées en dur, le dire, et indiquer « valeurs figées au <horodatage> ».

## Doc : bilan en tête du procès-verbal

Un Doc n'a pas de formules. Placer en tête de la section PV un tableau « Bilan » : mêmes statuts, nombre, part, passage précédent, écart, puis couverture exécutée, réussite sur exécutés, KO et Bloqué à revoir, régressions, indicateur si seuils définis, et un bloc « Temps » (durée totale, attente humaine, durée active Claude, durée moyenne par cas, référence manuelle, gain estimé ou « non calculable »). Les valeurs sont figées : écrire « figé au <horodatage> » et mettre à jour le bilan à la clôture du passage. Les décisions de l'opérateur ne s'y reflètent pas automatiquement.

## Angles morts du bilan à nommer à l'utilisateur
- Un taux de réussite élevé avec une couverture faible est trompeur : les Bloqué, Non testable et Non exécuté ne sont ni des réussites ni des échecs.
- OK signifie conforme au CDC tel qu'écrit, pas absence de défaut.
- Le bilan ne voit pas les défauts hors CDC : les observations hors CDC sont listées à part, sous le bilan.
- Un indicateur à trois états simplifie ; la décision se prend sur les KO, pas sur la couleur.
