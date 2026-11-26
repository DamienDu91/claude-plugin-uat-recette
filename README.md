# uat-recette

**Plugin Claude pour les recettes UAT (User Acceptance Testing) de sites web, front et back, pilotées par un cahier des charges Google Doc ou Sheet.**
*Claude plugin for UAT of websites (front end and back office), driven by a requirements spec in Google Docs or Sheets.*

[![Licence MIT](https://img.shields.io/badge/licence-MIT-blue.svg)](LICENSE)
![Version](https://img.shields.io/badge/version-0.4.0-informational)

## In English

`uat-recette` turns Claude into a UAT tester. Give it the requirements spec of a site (a Google Doc or Sheet); it asks for the context an expert would need (active Chrome tabs, front and back URLs, merchant or non-merchant site, what changed), runs each test case in the browser, and writes status, observed gap and screenshot back into the spec. A human operator then reviews and decides on corrective actions.

- Browser: built-in browser first, automatic switch to **Claude in Chrome** when a site blocks access.
- **Bilan** tab: OK / KO / Blocked counts, executed coverage, items awaiting human review, optional quality indicator based on thresholds you define.
- **Replay after fixes**: replays KO and Blocked cases plus regression sentinels, then reports fixed, persistent and regressed cases.
- **Time tracking**: Claude's active time, human waiting time, and an estimated productivity gain.
- Safety: writes only on whitelisted staging hosts after your explicit approval, never types passwords, never bypasses CAPTCHAs, append-only logging.

Skills are written in French. Prompts and results are in French by default.

## Installation

Dans Claude Code :

```
/plugin marketplace add DamienDu91/claude-plugin-uat-recette
/plugin install uat-recette@uat-recette-marketplace
```

Ou téléchargez le dépôt et ajoutez le dossier comme plugin local. Prérequis : connecteurs Google Docs / Sheets / Drive, et pour la bascule navigateur l'extension Claude in Chrome.

## Skills

| Skill | Rôle |
|---|---|
| `recette-init` | Contrôle qu'un CDC est exécutable (paramètres, cas vérifiables, porte staging) avant un premier passage |
| `recette-uat` | Lit le CDC, cadre la session, présente un plan pré-vol, exécute les cas, consigne statut + écart + captures, produit le bilan |
| `recette-rejeu` | Après correctifs : rejoue les KO et Bloqués (plus des sentinelles de non-régression), écrit un passage lié au précédent |

## Utilisation

- « Prépare la recette de ce CDC : <lien> » → `recette-init`
- « Lance la recette front et back de <lien> » → `recette-uat`
- « Les correctifs sont livrés, rejoue la recette de <lien> » → `recette-rejeu`

## Cadrage à chaque recette

Avant tout test, `recette-uat` demande, pour le site et le passage en cours : les onglets Chrome actifs (front et back, détectés puis confirmés), les URL et l'environnement, la nature du site (marchand ou non, B2C/B2B, vitrine, application), son objectif, la nature du changement testé, les rôles, marchés, intégrations, risques connus, critères de gravité et temps manuel de référence. Le cadrage est refait à chaque passage et consigné en tête du procès-verbal.

## Bilan en un coup d'œil

Chaque recette produit un onglet « Bilan recette » en première position du Google Sheet : nombre de OK, KO, Bloqué, Non testable, Non exécuté, part et barre visuelle, écart avec le passage précédent, couverture exécutée, KO et Bloqué restant à revoir, ventilation front/back et par priorité. Les compteurs sont des formules : ils suivent les décisions de l'opérateur. Un indicateur à trois états n'apparaît que si l'équipe a défini ses seuils dans les paramètres. Pour un Google Doc, le bilan est un tableau figé en tête du procès-verbal. Voir `skills/recette-uat/references/bilan.md`.

## Rejeu après correctif

Périmètre proposé : cas dont l'état courant est KO ou Bloqué, plus les cas OK de priorité Haute comme sentinelles contre les régressions (un rejeu limité aux KO n'en détecte aucune). Les cas rejetés comme faux positifs ou validés par l'opérateur sont exclus. Le bilan montre l'état consolidé (dernier statut de chaque cas, onglet « Consolidé ») et l'évolution : corrigés, persistants, régressions.

## Temps et gain de productivité

Chaque passage est chronométré par relevés d'heure (jamais estimés) : durée totale, attentes humaines, durée active Claude, durée par cas. Le gain estimé = temps manuel de référence (donné par l'opérateur) moins temps humain mobilisé (attentes + revue saisie). Sans référence, il est « non calculable ». La durée Claude n'est pas le temps humain économisé : voir `skills/recette-uat/references/chronometrage.md`.

## Principes

- Un CDC par site, avec un onglet « Paramètres de recette » (`skills/recette-uat/references/gabarit-cdc.md`).
- Écritures sur le site seulement si l'hôte figure dans la liste blanche `hotes_staging`, avec approbation explicite après le plan pré-vol.
- Consignation en ajout seulement, jamais d'écrasement du CDC. Colonne « Décision opérateur » laissée vide pour la revue humaine.
- Jamais de mot de passe saisi par l'outil : l'utilisateur se connecte lui-même.
- Statuts : OK, KO, Bloqué, Non testable, Non exécuté.

## Limites connues

- Les captures ne peuvent pas toujours être insérées dans le Doc/Sheet : référencées par nom de fichier, ou liées via Drive si le téléversement fonctionne.
- Pas de console, réseau, sévérité ni piste corrective par défaut.
- CAPTCHA et protections anti-bot : le cas passe en Bloqué.
- Les playbooks plateformes (Magento, Akeneo, Lovable, WordPress) sont des points de vigilance, pas des scripts.
- Un statut OK signifie conforme au CDC tel qu'écrit, pas absence de défaut. Une validation humaine reste nécessaire.

## Contribuer et licence

Voir [CONTRIBUTING.md](CONTRIBUTING.md) et [CHANGELOG.md](CHANGELOG.md). Licence [MIT](LICENSE).
