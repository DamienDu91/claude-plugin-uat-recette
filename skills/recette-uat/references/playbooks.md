# Points de vigilance par plateforme

Ces notes orientent l'exécution. Elles ne remplacent jamais le résultat attendu du CDC. En cas de doute sur une interface récente, observer la page plutôt que se fier à ces notes.

## Magento / Adobe Commerce

Front : parcours catalogue (listing, filtres, tri, pagination), fiche produit (prix, variantes, stock, prix dégressifs), panier, checkout invité et connecté, codes promo, multi-vues magasin (langue, devise), recherche, comptes clients, formulaires.
Back : grille produits, fiches produit (attributs, catégories, images), commandes (statut, facture, expédition), clients, règles de prix, CMS (pages, blocs), configuration par portée (site / vue de magasin).
Vigilance :
- Les caches et l'indexation retardent l'affichage : après une écriture en back, attendre ou vérifier l'état d'indexation avant de conclure à un KO. Un front inchangé juste après une écriture n'est un KO qu'après un second passage différé.
- Les écritures de configuration dépendent de la portée sélectionnée : vérifier la portée affichée.
- Une commande de test peut déclencher e-mails, ERP, paiement : voir `emails_effets_de_bord` et `exclusions`.
- Clés d'administration dans les URL : ne pas les copier dans le PV.

## Akeneo PIM

Back-office produit : familles, attributs, catégories, fiches produit et modèles, complétude, canaux, locales, jeux de données, exports/imports, droits par rôle.
Vigilance :
- Les imports/exports sont des tâches asynchrones : attendre l'état terminal avant de conclure. Les téléchargements exigent une permission explicite.
- La complétude se calcule par canal et locale : noter lesquels sont sélectionnés dans l'écart.
- Les publications vers le front (connecteur, sync) peuvent être différées : vérifier côté front uniquement si le cas le demande, avec délai explicite.
- Ne pas modifier de données sans le préfixe de test ; ne pas lancer d'import massif sans confirmation individuelle.

## Lovable / React (SPA)

Front : navigation côté client (pas de rechargement), états de chargement, formulaires, authentification, rôles, responsive, routes profondes (rechargement sur une URL interne), 404.
Back (si backend Supabase) : écrans d'administration de l'app, politiques d'accès par rôle.
Vigilance :
- Le contenu se charge après le premier rendu : attendre l'état stable avant la capture et la comparaison.
- Tester le rechargement direct d'une route interne en plus de la navigation interne.
- Les prévisualisations Lovable ne sont pas la production : l'hôte exact doit figurer dans `hotes_staging`.
- Les console errors ne sont lues que si un cas le demande.

## WordPress / autre CMS

Front : pages, menus, formulaires de contact, recherche, médias, SEO visible (titre, méta si le cas l'exige), cookies/consentement, multilingue.
Back : édition de page/article, médias, utilisateurs et rôles, plugins/réglages visés par le cas.
Vigilance :
- Un éditeur de blocs peut être lent à charger : attendre l'état stable avant d'agir.
- La publication d'un contenu en staging reste une écriture : porte staging et préfixe de test.
- Les mises à jour de plugins/thème et les changements de réglages globaux ne sont jamais joués sans cas explicite et confirmation individuelle.
- Formulaires de contact : vérifier que l'envoi ne part pas vers une vraie boîte (`emails_effets_de_bord`).

## Transverse

- Responsive : vérifier la largeur de fenêtre demandée par le cas, redimensionner la fenêtre du navigateur au besoin, noter la largeur utilisée.
- Accessibilité, performance, SEO technique : hors périmètre sauf cas explicite du CDC. Ne pas ajouter de constats non demandés ; si un défaut manifeste est vu en passant, le signaler dans la synthèse comme observation hors CDC, sans statut.
