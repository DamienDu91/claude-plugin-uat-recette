# Chronométrage de la recette

But : donner à l'opérateur une mesure fiable du temps passé par Claude, pour estimer le temps humain économisé. Claude n'a pas d'horloge propre : relever l'heure avec l'outil d'heure courante (`current_time`), sinon `date` en shell. Ne jamais estimer une durée de tête.

## Relevés obligatoires (heure de Paris, à la seconde)

| Repère | Moment |
|---|---|
| T0 début | Tout début du cadrage (étape 0) |
| T1 fin cadrage | Cadrage confirmé par l'utilisateur |
| T2 début exécution | Après le oui explicite au plan pré-vol |
| Fin de chaque cas | Juste après la consignation du cas |
| T3 fin | Après relecture du bilan, avant la réponse finale |

Durée d'un cas = heure de fin du cas − heure de fin du cas précédent (pour le premier : T2). Elle inclut déplacement, capture et consignation : c'est volontaire, c'est le coût réel du cas.

## Attentes humaines (à exclure du temps Claude)

À chaque fois que Claude doit attendre l'utilisateur, relever l'heure juste avant de poser la question ou la demande, et l'heure de la réponse. Si le message de l'utilisateur porte son heure d'envoi dans le contexte, l'utiliser comme heure de réponse ; sinon relever l'heure à la réception. Motifs types : réponses de cadrage, validation du plan pré-vol, connexion à faire par l'utilisateur, confirmation d'une action irréversible, question de clarification.

Tenir une liste : motif, début, fin, durée.

## Calculs

- **Durée totale écoulée** = T3 − T0.
- **Attente humaine** = somme des attentes humaines.
- **Durée active Claude** = durée totale écoulée − attente humaine.
- **Durée moyenne par cas** = somme des durées de cas ÷ nombre de cas exécutés (OK, KO, Bloqué).
- Les cas Non testable et Non exécuté n'ont pas de durée d'exécution : laisser la cellule vide.

## Mesure du gain : ce qu'il faut dire honnêtement

La durée Claude n'est pas le temps humain économisé. Un humain qui fait la même recette ne met pas « autant de temps que Claude » : il met son propre temps. Le gain pour l'opérateur est :

**temps manuel de référence − temps humain mobilisé**, où le temps humain mobilisé = attente humaine pendant la recette (cadrage, approbations, connexions) + temps de revue des résultats.

- Le **temps manuel de référence** est une estimation donnée par l'opérateur (par recette, ou en minutes par cas). La demander au cadrage. Ne jamais l'inventer ; sans référence, afficher la durée Claude seule et « gain non calculable ».
- Le **temps de revue** est saisi par l'opérateur après coup dans le bilan. Le plugin ne peut pas le mesurer.
- Présenter la durée Claude comme un indicateur de délai et de coût d'exécution, et le gain humain comme une estimation qui dépend de la référence fournie.

## Angles morts à nommer
- La référence manuelle est déclarative et souvent optimiste ou pessimiste : la noter comme hypothèse dans le bilan.
- Les premiers passages d'un site coûtent plus cher (cadrage, découverte) que les suivants : comparer les passages entre eux, ne pas extrapoler un seul chiffre.
- La durée dépend de la latence des sites et des outils, pas seulement du travail : ne pas en déduire la charge intrinsèque d'un cas.
- Un gain de temps n'est pas un gain de qualité : ne jamais présenter l'un comme preuve de l'autre.
