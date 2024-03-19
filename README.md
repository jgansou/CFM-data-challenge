# CFM-data-challenge

## But
L'objectif de ce challenge est d'essayer d'identifier, à partir d'une séquence de données boursières, quelle est l'action correspondante. On considère donc une tâche de classification. Les données comprennent chaque mise à jour atomique du carnet d'ordres, donnant des informations sur les meilleurs prix d'achat et de vente, les transactions qui ont pu avoir lieu, les ordres qui ont été placés dans le carnet ou annulés. Le carnet d'ordres est un carnet d'ordres agrégé, construit à partir de plusieurs bourses où il est possible d’acheter et de vendre les mêmes actions. Bien que les données semblent très peu descriptives et anonymes, nous nous attendons à ce qu'elles contiennent des indices permettant de savoir à quel titre elles correspondent. Il peut s'agir du spread moyen, des quantités typiques d'actions à l'achat ou à la vente, de la fréquence des transactions, de la répartition des transactions entre les lieux où l'action est négociée, etc. Il y a beaucoup d’informations pour aider les participants.

## Description des données
X : les données sont composées de séquences de 100 mises-à-jour atomiques consécutives du carnet d’ordre, prises à des instants aléatoires de la journée. Plus précisément, X contient 20 séquences par action et par jour. Les données contiennent 504 jours (environ 2 ans) et concernent 24 titres. Le fichier comprend donc 100 x 20 x 505 x 24 = 24240000 lignes. Les colonnes correspondent aux éléments suivants :

- obs_id : identifie de façon unique une séquence de 100 mises-à-jour du carnet d’ordre pour un instant de la journée et un titre aléatoire ;

- venue : la place boursière où l’évènement a eu lieu, par ex. NASDAQ ou BATY, encodée par un entier.

- action : le type d’évènement du carnet d’ordre ayant eu lieu (‘A’, ‘D’ ou ‘U’) . ‘A’ correspond à un ajout de volume au carnet sous la forme d’un nouvel ordre. ‘D’ signifie qu’un ordre a été supprimé du carnet et ‘U’ correspond à la mise-à-jour d’un ordre.

- order_id : les données boursières sont ‘Level 3’ ou ‘Market-by-Order’, ce qui signifie que chaque mise-à-jour fournit un identifiant unique pour l’ordre concerné. Cela signifie que l’on peut suivre l’évolution d’un ordre individuel. S’il a été placé plus tôt avec un type ‘A’, on peut le voir être supprimé par le même acteur sous la forme d’un évènement de type ‘D’ présentant le même identifiant. Notez néanmoins que les identifiants des ordres ont été masqués. En effet, le premier ordre référencé dans chaque séquence de 100 évènements se voit attribué l’identifiant 0. Si l’identifiant 0 apparaît à nouveau dans un évènement, vous saurez que c’est le même ordre qui a été affecté ;

- side : le côté du carnet d’ordre où l’évènement s’est produit (‘A’ ou ‘B’) ;

- price : le prix de l’ordre concerné ;

- bid : le meilleur prix à l’achat ;

- ask : le meilleur prix à la vente ;

- bid_size : le volume d’ordres au meilleur prix d’achat du carnet agrégé ;

- ask_size : le volume d’ordres au meilleur prix de vente du carnet agrégé ;

- flux : le changement du carnet d’ordre affecté par l’évènement, i.e. si le volume pour un niveau croît ou décroît à cause de l’évènement ;

- trade : un booléen vrai ou faux pour indiquer si un évènement de suppression ou de mise-à-jour est dû à une vente ou à une annulation.

Comme le prix en lui-même constitue un indice particulièrement important, on soustrait le meilleur prix d’achat pour le premier évènement de chaque séquence de 100 aux colonnes ‘price’, ‘bid’ et ‘ask’.

Y : les labels du jeu de données correspondent au eqt_code_cat. Pour la construction du jeu de données d’entraînement, on utilise un entier entre 0 et 23 qui identifie l’action concernée.

Les données d’entraînement et de test sont issues de deux périodes différentes.
