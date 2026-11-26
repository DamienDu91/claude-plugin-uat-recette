# Structure attendue du CDC

Le plugin lit un CDC par site. Il tolère des formats libres mais fonctionne bien mieux avec cette structure. Pour un CDC qui n'y ressemble pas, appliquer la tolérance décrite en bas et le signaler.

## Onglet / section « Paramètres de recette »

Clé / valeur, une par ligne. Aucun secret : pas de mot de passe, pas de clé.

| Clé | Exemple | Rôle |
|---|---|---|
| site | exemple.fr | Nom du site |
| plateforme | magento / akeneo / lovable / wordpress / autre | Choix du playbook |
| url_front_staging | https://staging.exemple.fr | Base front de recette |
| url_back_staging | https://staging.exemple.fr/admin | Base back de recette |
| url_prod | https://www.exemple.fr | Référence, jamais écrite |
| hotes_staging | staging.exemple.fr; recette.exemple.fr | Liste blanche exacte pour la porte staging (vide = aucune écriture) |
| navigateur | auto / integre / chrome | Voir navigateur.md |
| roles_comptes | admin-catalogue; client-test; commercial | Noms des rôles à utiliser, sans identifiants |
| prefixe_donnees_test | UAT- | Préfixe obligatoire pour toute donnée créée |
| dossier_captures | lien du dossier Drive (optionnel) | Destination des captures |
| exclusions | paiement réel; envoi e-mails clients | Ce qui ne doit jamais être joué |
| emails_effets_de_bord | oui / non / désactivés en staging | Savoir si une écriture envoie de vrais e-mails |
| captcha_connu | oui / non | Anticiper les Bloqués |
| seuil_ko_prioritaire_max | 0 | Nombre maximal de KO de priorité haute pour l'indicateur du bilan |
| seuil_reussite_min | 95 % | Réussite minimale sur cas exécutés |
| seuil_couverture_min | 90 % | Couverture exécutée minimale |
| libelles_priorite | P1=Haute; P2=Moyenne; P3=Basse | Correspondance vers Haute / Moyenne / Basse |

Les seuils sont fixés par l'équipe, jamais par le plugin. Sans seuils, le bilan affiche les chiffres sans indicateur.

## Onglet / section « Cas de test »

Une ligne ou un bloc par cas :

| ID | Périmètre (front/back) | Priorité | Préconditions | Étapes | Résultat attendu |
|---|---|---|---|---|---|

Règles d'écriture conseillées pour l'auteur du CDC (à rappeler à l'utilisateur si le CDC est faible) :
- un résultat attendu vérifiable : texte exact, valeur, présence d'un élément, statut, redirection ;
- pas d'« affichage correct » ou « fonctionne bien » seuls : ces cas deviennent **Non testable** ;
- un identifiant stable par cas (ne pas renuméroter entre deux passages) ;
- les cas qui écrivent sont repérables (colonne « Écrit ? » ou mention explicite).

## Tolérance

- CDC en prose : extraire les cas implicites, les lister dans le plan pré-vol avec un identifiant provisoire (`TMP-01`…) et demander validation de cette extraction avant d'exécuter.
- Colonnes nommées autrement : mapper et dire le mapping retenu en une ligne.
- Pas de paramètres de recette : les demander (au minimum URL, hôtes staging, rôles). Sans hôte staging déclaré, aucune écriture.
