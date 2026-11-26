# Choix du navigateur et bascule vers Claude in Chrome

## Principe

Deux navigateurs possibles :
- **Navigateur intégré** (outils `Claude_Browser__*`, volet du Claude desktop) : par défaut en `auto`.
- **Claude in Chrome** (outils `claude-in-chrome__*`) : le vrai Chrome de l'utilisateur, avec ses sessions déjà ouvertes. À privilégier pour le back-office, les SSO et tout site qui bloque le navigateur intégré.

Le paramètre `navigateur` du CDC vaut `auto` (défaut), `integre` ou `chrome`. En `integre` ou `chrome`, ne pas basculer : si le navigateur imposé est indisponible, le dire et demander.

Ne pas annoncer le choix de navigateur en détail ; le consigner dans le PV.

## Mode auto : quand basculer

Basculer vers Claude in Chrome, pour le site concerné, uniquement sur un **blocage d'accès** constaté :
- la navigation échoue ou est refusée alors que l'URL est valide (domaine non accessible depuis le volet intégré, restriction du site) ;
- la page affiche un refus d'accès, un blocage WAF ou une page « navigateur non supporté » ;
- une connexion est requise et le navigateur intégré n'a pas de session, alors que Chrome en a une (SSO, back-office déjà ouvert) ;
- l'utilisateur indique que le site n'est accessible que depuis son Chrome (VPN, liste d'IP, extension).

Ne pas basculer pour :
- un cas KO (un écart n'est pas un problème de navigateur) ;
- un chargement lent isolé (réessayer une fois) ;
- un CAPTCHA ou un challenge anti-bot : ne pas les résoudre, ni dans un navigateur ni dans l'autre. Si le challenge apparaît aussi dans Chrome : **Bloqué**.

Une fois la bascule faite pour un site, rester sur Chrome pour toute la recette de ce site (pas de va-et-vient entre navigateurs). Consigner dans le PV : navigateur utilisé, motif de la bascule, cas concernés.

## Mise en route de Claude in Chrome

1. Charger les outils en une seule recherche d'outils différés :
   `select:mcp__claude-in-chrome__tabs_context_mcp,mcp__claude-in-chrome__navigate,mcp__claude-in-chrome__computer,mcp__claude-in-chrome__read_page,mcp__claude-in-chrome__tabs_create_mcp,mcp__claude-in-chrome__tabs_close_mcp`
   Ajouter `read_console_messages` et `read_network_requests` seulement si un cas l'exige explicitement, `form_input` pour les formulaires, `find` pour localiser un élément.
2. Appeler `tabs_context_mcp` en premier. Ne jamais réutiliser d'identifiant d'onglet d'une session précédente. Les onglets front et back désignés par l'utilisateur lors du cadrage (voir `cadrage.md`) peuvent être utilisés ; tout autre onglet reste intact, et un nouvel onglet est ouvert pour le reste.
3. Ne pas déclencher de boîtes de dialogue JavaScript (alert, confirm, prompt) : elles bloquent l'extension. Si un cas en provoque une, prévenir l'utilisateur avant.
4. Après 2 à 3 échecs répétés d'un outil du navigateur (page qui ne charge pas, éléments qui ne répondent pas, extension muette), arrêter, expliquer ce qui a été tenté, demander. Ne pas boucler.

## Si aucun navigateur n'est disponible

Dire lequel manque (volet intégré fermé, Chrome fermé ou extension absente), demander à l'utilisateur de l'ouvrir, ne pas improviser une autre méthode de test. Aucun résultat ne doit être déduit sans avoir vu la page.

## Preuves d'écran

- Navigateur intégré ou Chrome : capture de l'onglet de test, au moment utile (après chargement complet, élément concerné visible).
- Un cas = au minimum une capture de l'état final. KO = en plus l'état où l'écart est visible.
- Ne pas enregistrer de GIF sauf demande.
