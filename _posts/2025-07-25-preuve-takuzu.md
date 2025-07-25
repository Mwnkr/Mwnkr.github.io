Absolument. Voici la conversion de votre document LaTeX en Markdown, optimisée pour être lisible sur des plateformes comme GitHub Pages.

***

# Une aventure combinatoire à travers le puzzle Tango
**Auteur :** LAMOUCHI Mairwane
**Date :** 20 juillet 2025

> ### Abstract
> Tout a commencé aujourd'hui en jouant à "Tango", l'un des nouveaux puzzles logiques de LinkedIn, clairement basé sur le jeu bien connu "Takuzu" ou "Binero" en français. Les règles sont simples : remplir une grille de 6×6 avec deux symboles (ici, des soleils et des lunes) de sorte que chacun apparaisse trois fois par ligne et par colonne, et que pas plus de deux symboles identiques n'apparaissent consécutivement. Cette contrainte a soulevé une question combinatoire plus générale : pour une taille paire 2n, combien de lignes ou de colonnes valides sont réellement possibles ?
>
> Je m'attendais à une réponse rapide, mais le problème s'est avéré plus subtil que prévu. Ce document retrace le processus que j'ai suivi pour dériver une formule pour $a(n)$, le nombre de chaînes binaires avec n zéros et n uns qui évitent les séries de plus de deux chiffres identiques.
>
> Il s'avère que la formule est (évidemment) déjà documentée dans l'Encyclopédie en ligne des suites d'entiers comme la suite [A003440](https://oeis.org/A003440). Mais comme mon objectif était de la trouver par moi-même, j'ai pensé que le partage du voyage étape par étape avait toujours de la valeur. Ce document est ce voyage : un chemin complet et indépendant vers la solution.

***

## Alors, quel est le défi exact ?

Les règles du jeu ont inspiré un défi mathématique très spécifique. Pour formuler clairement le problème — et, comme je l'ai découvert plus tard, pour m'aligner sur la notation standard de l'OEIS — j'ai décidé de définir $a(n)$ comme la réponse à ma question :

*Pour une séquence de longueur 2n, quel est le nombre total de façons d'arranger n zéros et n uns, avec la condition stricte qu'il ne peut y avoir trois chiffres identiques ou plus à la suite (donc, `000` et `111` sont interdits) ?*

Le but de ce document est de prouver que la réponse à cette question est donnée par cette formule hideuse :
$$a(n) = \sum_{i=0}^{\lfloor n/2 \rfloor} \left( 2 \binom{n-i}{i}^2 + \binom{n-i}{i}\binom{n-i-1}{i+1} + \binom{n-i}{i}\binom{n-i+1}{i-1} \right)$$

***

## Le plan : Décomposer en blocs

J'ai réalisé que toute séquence valide devait être construite à partir de seulement quatre "briques" de base :
* Un zéro simple : `0`
* Un double zéro : `00`
* Un un simple : `1`
* Un double un : `11`

La règle clé est qu'il faut alterner entre les "blocs de zéros" et les "blocs de uns". Disons que nous avons $k_0$ blocs de zéros et $k_1$ blocs de uns au total. En raison de cette alternance, leurs comptes doivent être presque identiques.

Ils peuvent différer d'au plus un, ce qui signifie $|k_0 - k_1| \le 1$. Cela peut sembler inutile à préciser, mais nous serons heureux de l'avoir fait.

### Compter les briques de construction
Nous devons compter. La question est : devrions-nous travailler avec les blocs simples ou doubles ? Eh bien, le nombre de blocs simples doit avoir la même parité que $n$, alors que cela ne s'applique pas au nombre de blocs doubles. Nous allons donc définir le nombre de blocs doubles, ce qui induira le nombre de blocs simples.

Disons que nous utilisons $i$ blocs double-zéro (`00`). Pour obtenir notre total de $n$ zéros, nous avons alors besoin de $n-2i$ blocs simple-zéro (`0`). Cela nous donne un total de $k_0 = i + (n-2i) = n-i$ blocs de zéros. La question devient alors : de combien de manières pouvons-nous les arranger ? C'est un problème de combinatoire classique : choisir $i$ emplacements pour les blocs doubles parmi $n-i$ emplacements au total. La réponse est $\binom{n-i}{i}$.

La même logique s'applique exactement aux blocs de uns. Si nous utilisons $j$ blocs double-un, nous pouvons former notre ensemble de blocs de uns de $\binom{n-j}{j}$ manières.

***

## Tout assembler

Passons maintenant à l'étape finale : l'assemblage de la séquence complète. Le décompte total, $a(n)$, sera la somme de toutes les manières possibles de combiner nos ensembles de blocs de zéros et de blocs de uns. Cette intuition cruciale, $|k_0 - k_1| \le 1$, simplifie énormément les choses. Cela signifie que nous n'avons que trois scénarios à considérer.

> **Note :** Pour rendre la formule générale plus fluide, nous supposerons dans ce qui suit que $\binom{n}{k} = 0$ si $k > n$ ou $k < 0$. Cela nous permet d'écrire des sommes plus propres sans trop nous soucier des bornes exactes.

### Scénario 1 : Un nombre égal de blocs de zéros et de uns
C'est le cas où $k_0 = k_1$, ce qui signifie $n-i = n-j \implies i = j$.
* **Les briques de construction** : Nous avons $\binom{n-i}{i}$ façons de créer les blocs de zéros et $\binom{n-i}{i}$ façons pour les blocs de uns.
* **Assemblage** : Avec le même nombre de blocs, nous avons deux façons de commencer la séquence : soit avec un bloc de zéros, soit avec un bloc de uns. Nous multiplions donc notre compte par 2.
* **Ce morceau de la formule** : $\sum_{i=0}^{\lfloor n/2 \rfloor} 2 \binom{n-i}{i}^2$

### Scénario 2 : Un bloc de zéros de plus que de blocs de uns
Cela se produit lorsque $k_0 = k_1 + 1$. Puisque $k_0 = n-i$ et $k_1 = n-j$, nous obtenons $n-i = (n-j)+1 \implies j = i+1$.
* **Les briques de construction** : $\binom{n-i}{i}$ façons pour les zéros et $\binom{n-(i+1)}{i+1} = \binom{n-i-1}{i+1}$ pour les uns.
* **Assemblage** : Pour avoir un bloc de zéros supplémentaire, la séquence doit à la fois commencer et se terminer par un bloc de zéros (`0...1...0`). Cela ne laisse qu'une seule façon de les arranger.
* **Ce morceau de la formule** : $\sum_{i=0}^{\lfloor n/2 \rfloor} \binom{n-i}{i} \binom{n-i-1}{i+1}$

### Scénario 3 : Un bloc de uns de plus que de blocs de zéros
L'image miroir du cas précédent. Ici, $k_1 = k_0 + 1$, ce qui signifie $n-j = (n-i)+1 \implies j = i-1$.
* **Les briques de construction** : $\binom{n-i}{i}$ façons pour les zéros et $\binom{n-(i-1)}{i-1} = \binom{n-i+1}{i-1}$ pour les uns.
* **Assemblage** : De même, la séquence doit commencer et se terminer par un bloc de uns (`1...0...1`). Encore une fois, une seule façon de le faire.
* **Ce morceau de la formule** : $\sum_{i=0}^{\lfloor n/2 \rfloor} \binom{n-i}{i} \binom{n-i+1}{i-1}$

***

## Le grand final : La formule complète
Parce que ces trois scénarios sont mutuellement exclusifs et couvrent toutes les possibilités valides, il ne nous reste plus qu'à les additionner.

Cela nous donne la formule finale et complète :
$$a(n) = \sum_{i=0}^{\lfloor n/2 \rfloor} \left( 2 \binom{n-i}{i}^2 + \binom{n-i}{i}\binom{n-i-1}{i+1} + \binom{n-i}{i}\binom{n-i+1}{i-1} \right)$$

***

### Une dernière pensée

Je terminerai par une note personnelle : pour un puzzle binaire aussi simple, la formule finale n'est pas ce que j'appellerais élégante. Je m'attendais à quelque chose de propre, comme une réponse basée sur une puissance de 2 (au moins calculable mentalement pour les premiers termes), pas cette somme horrible. Cela montre simplement que même une configuration simple peut mener à un résultat étonnamment compliqué, et cela en soi valait bien mon temps.
