---
title: TD5
subtitle: OverConstraint, Fluid Layout, Responsive design, Mobile first...
layout: tutorial
---

## Introduction

Les mobiles, les tablettes sont appareils récents et 
concevoir un site internet ou une application Web s'adressant à tous ces
médias n'est pas une tache triviale. Voici un aperçu des nombreuses solutions existantes :

* Une solution en pur CSS. En effet, le CSS est le langage de base pour la mise
  en page des pages Web. Il est donc en constante évolution (CSS3 en cours
  d'élaboration) pour s'adapter aux nouveau besoins.

* Coder deux sites en HTML/CSS : un pour les ordinateurs et un pour les
   smartphones (et un pour les tablettes ? et un pour les smartwatch ?). Exemple
   [lemonde.fr](http://lemonde.fr) et [mobile.lemonde.fr](http://mobile.lemonde.fr).

* Des applications natives pour chaque système (Android, iOS, Windows Phone, ...)

* L'utilisation de la puissance de JavaScript pour faire des mises en page
  adaptative. 
  <!-- (et dire du mal de Javascript ensuite...) -->

* Faire le site en Flash (très mauvaise idée car il n'est plus supporté sur
  Android ni iOS).

 <!-- - utilisation de Phone Gap pour faire une application hybride (le site web est -->
 <!--   "converti" en application, on a un seul code source) -->
 <!-- - React Native,... -->

Plus pour nous positionner dans cette jungle que par purisme, nous prendrons deux hypothèses de départ :

* *"Il n'y a pas besoin d'applications pour cela !"* : Pas besoin de faire du
   natif Android ou iOS.
* *"Il n'y a pas besoin de faire deux sites web pour cela!"* : On s'interdit de
   faire un site Web pour mobile et un pour ordinateur.


Nous adopterons dans ce TD une approche itérative, en rajoutant au fur et à
mesure des contraintes pour arriver à ce qui se fait aujourd'hui dans le
responsive design.


## CSS2

Certaines propriétés n'ont pas attendu ni les nouveaux médias ni le CSS3 pour
s'imposer aux développeurs.  Elles prenaient déjà tout leur sens sur des sites
particulièrement fournis et/ou sur des petits écrans 15 pouces.

### Les pourcentages '%'

On peut commmencer par exprimer toutes les tailles en relatif, en prenant comme
référence la largeur de l'écran. C'est ce que nous avons déjà fait en utilisant
des dimensions en `%`.

<div class="exercise">

1. Donnez à `<body>` la `width` de `100%`.
   <!-- et les marges doivent rester à 0 ou à auto -->
1. Enlevez au besoin la marge de gauche de `10%` sur le `<aside>` et changez les
   dimensions relatives de `<article>` et `<aside>` à respectivement `67%` et
   `33%`.

</div>


**Note :** Si adapter les largeurs en pourcentage marche bien, ce n'est pas le
cas des hauteurs.  Cela est dû au fait que la largeur de la page est connue
(c'est la largeur du navigateur) mais sa hauteur ne l'est pas encore
... (puisque la page n'est pas encore affichée et que la hauteur va dépendre de
la quantité de contenu). Autrement dit, la hauteur de la zone du navigateur où
s'affiche la page (`viewport height`) n'est pas forcément la hauteur de la page
[^somesamplefootnote] (c'est lié à la présence d'un ascenceur à droite pour
descendre dans la page).

[^somesamplefootnote]: L'unité `vh` permet maintenant de définir une taille de 0 à 100 relative au viewport.


### `max-with` et `min-with`


Les règles précédentes permettent d'avoir un rapport homogène, mais pas un rendu
optimal :

 - sur de petits écrans d'ordinateurs, des éléments ne peuvent pas être
   correctement affichés (des images, des colonnes, ...)
 - sur de très grands écrans, le texte devient illisible : les yeux fatiguent à
   cause des lignes trop longues.
 
Pour contraindre les dimensions maximales et minimales, nous utiliserons
`max-width`, `max-height`, `min-width` et `min-height`, qui prennent le même
type de valeur que `width` et `height`.


<div class="exercise">
1. Ajoutez une limite minimale pour les photos de Chuck Norris à `150px`, (et encore, mieux vaut ne pas en parler à Chuck).
<!-- ATTENTION : on n'a pas encore fixé la taille de l'image comme étant
relative. Veux-tu passer plus de choses en taille relative ? -->

 1. Ajoutez une limite maximum de largeur à l'`<article>` et à l'`<aside>` de `500px` et de `250px`.
 1. Ajoutez une limite minimum de largeur à l'`<article>` et à `<aside>` de `200px` et `150px`.
 <!-- ATTENTION 150px pas assez pour afficher les liens <a> du aside -->
 <!-- Et quelle taille pour afficher le tableau comparatif du article ? -->
</div>


### Overconstraint

Évidement lorsque l'on joue avec des `min-width` et un petit écran à un moment
çà dépasse.

Il va falloir donc utiliser des règles subtiles pour savoir qui relachent la
contrainte entre `<aside>` et `<article>`.

On va utiliser FlexBox pour exprimer cela en se servant de
[cette super page sur FlexBox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/).
Grosso-modo nous avons déjà vu les règles de la colonne de gauche de cette
page : c'est celles qui s'appliquent aux parent.  Nous allons maintenant
découvir les règles de droites : celles qui s'appliquent aux éléments. Regardez
particulièrement les propriétés `flex-shrink`, `flex-grow`. Nous ne toucherons
pas à `flex-basis` (qui gardera donc son comportement par défaut `auto`).

Nous n'en disons pas plus sur cette page volontairement : le travail d'un
développeur Web consiste pour une bonne partie chercher des solutions sur le
Web. (Nous restons bien sûr à votre disposition pour parler de votre
compréhension.)

<!-- Attention, le comportement de l'exercice précédent est déjà déformé par les
propriétés par défaut flex-grow:0; flex-shrink:1 -->

<!-- Voilà comme je le comprend : -->
<!-- D'abord on calcule la largeur des blocs en utilisant width -->
<!-- Enfin on réparti l'espace restant sur la largeur -->
<!-- Puis on applique les contraintes min-max-width -->

<!-- ANCIEN EXERCICE QUI NE ME SEMBLE PAS FAISABLE -->
<!--  1. Quand l'écran est trop petit pour afficher `<article>` et `<aside>` avec -->
<!--     leurs contraintes de tailles minimales de la section précente, fait en sorte -->
<!--     qu'`<article>` prenne les deux tiers de la place et `<aside>` le reste. -->
<!--  1. Quand l'écran est trop grand, faites en sorte qu'`<article>` soit 3 fois -->
<!--     plus grand qu'`<aside>`. -->

<!-- NOUVEL EXERCICE : ON EN PARLE -->
<div class="exercise">

1. Implémentez le comportement suivant :  
Changez les largeurs de `<article>` et `<aside>` pour qu'elles
soient par défaut de `300px` et `200px` et que :

   1. Quand l'écran est trop petit pour afficher `<article>` et `<aside>`,
    `<article>` diminuera de deux tiers de la largeur à supprimer et `<aside>`
    diminuera du reste.
	
   1. Quand l'écran est trop grand, faites en sorte que l'espace restant soit
    réparti entre `<article>` et `<aside>` de telle sorte que l'espace gagné par
    `<article>` soit 3 fois plus grand que celui gagné par `<aside>`.

   Bien sûr, toutes ces largeurs respecteront les anciennes contraintes `min-width` et `max-width`.

   <!-- Utiliser le device mode pour forcer facilement la largeur de la
   fenetre. Mais je ne comprends pas toujours comment ce mode fonctionne
   À cause de l'ascenceur déjà -->

2. Dimensionnez votre fenêtre de sorte à ce que `<body>` ait une largeur de
   `620px`. Inspectez les largeurs de `<article>` et `<aside>`. Vérifiez que
   votre comportement satisfait le cahier des charges.

   <!-- Réponse : article 390 px et aside 230px car 120px de rab réparti en 30px de rab par unité -->

3. Dimensionnez votre fenêtre de sorte à ce que `<body>` ait une largeur de
   `380px`. Inspectez les largeurs de `<article>` et `<aside>`. Vérifiez que
   votre comportement satisfait le cahier des charges.

   <!-- Réponse : article 220 px et aside 160px car 120px à gagner réparti en 40px à enlever par unité   -->
   <!-- Je ne sais pas pourquoi chromium fait 210 / 170 ??? -->
   <!-- EXERCICE 3.3 buggé à cause de flex-shrink buggé ??? -->
   
<!--
ATTENTION :
* ne pas utiliser le raccourci flex: 3 2; ne va pas car il force flex-basis:0% au
lieu de flex-basis: auto;
-->

</div>


### Problèmes plus complexes


Il arrive un moment où diminuer encore la taille n'a plus de sens. Il faut
prendre des mesures draconiennes, par exemple passer `<aside>` sous `<article>`
ou carrément supprimer `<aside>`.

Les règles CSS déjà vues en TDs ne permettent pas de coder ce genre de
comportement. Il nous faut une façon d'écrire du CSS qui ne sera valide que dans
des cas précis. Nous verrons dans la prochaine section comment cela est possible
en CSS3.

## Que fait mon navigateur web sur téléphone par défaut pour un site ?



Comment peut on à moindre coût rendre tous les sites internets compatibles mobile ?


Ue solution pas chère est faite par défaut :

 * Générer le site sur un écran viruel 800 par 600 
 * Faire un scalling pour faire "rentrer cela" dans l'écran du smartphone
 * Considérer que l'utilisateur connait le pinch to zoom pour naviguer dans le site.

 Et çà marche ! 

Cela marche mais on ne peut pas dire que cela est forcément le mieux.
L'utilisateur mobile n'a pas la même attente que l'utilisateur sur ordinateur, il veut avoir accès rapidement aux informations essentielles, sans fioritures.
On imagine que sur une smart watch par exemple un site comme méteo france serait largement plus dépouillé (sur une smartwatch elle afficherait un nuage ou un soleil et la temperature par exemple).

Typiquement nous voulons enlever des parties entières du sites suivant la taille de l'écran.


La première chose à faire donc c'est de demander aux navigateurs de ne plus faire l'algo précédent (puisqu'on va gérer nous-mêmes) dnas la balise `<head>` :

~~~
 <meta name="viewport" content="width=device-width, initial-scale=1">
~~~
{:.html}
<div class="exercise">
  Ajouter cette instruction au site de Chuck Norris et visualisez avec Chrome en choissisant un smartphone. Que constatez vous ?
</div>


Bon vous devez constatez que l'algo ne se fait plus. En gros le navigateur n'essait plus d'être intelligent : il va falloir prendre le relai.



## Les outils pour travailler ?

Outil de dévellopement Chrome !
Jusqu'ici on pouvait considéré que tous les outils de dévellopement d'Internet EXplorer Firefox et Chrome étaient égaux.
En fait celui de Chrome était déjà un peu meilleur :

  * auto completion des règles css ajoutées dynamiquement,
  * liste déroulante des valeurs possibles des champs,
  * plus rapide, plus stable (un process par onglet)
  * etc.

Suivant les habitudes des devellopeurs cela peut être plus ou moins sujet à contreverse, Mais pour le Reponsive design il n'y a pas photo : 
Chrome (ou son pendant libre Chromium) est <strong>vraiment</strong> votre "best-friend-ever".

* Si vous n'avez pas Chrome ou Chromium, ou si en appuyant sur F12 vous ne voyez pas la petite icone :
<img src="{{site.baseurl}}/assets/phone-responsive.png" alt="Outil opur mobile" style="margin: 0 auto;display: block;"> 

allez sur le [site de l'IUT](https://iutdepinfo.iutmontp.univ-montp2.fr/), identifiez vous et téléchargez directement [le lien](https://infolimon.iutmontp.univ-montp2.fr/public/windows/chrome-win32.zip).



## La solution technique CSS3 :  les media queries

Les
media queries sont un jeu d'options ajoutées à la norme CSS 3 et qui permettent
de définir des règles CSS qui ne s'appliqueront que sous certaines conditions
spécifiques.

Il existe deux manières de faire appel à une media query:

* charger un fichier CSS spécifique en entier, uniquement dans certaines
  conditions. Il faut ajouter à l'élément “head” de la page web une déclaration
  de fichier CSS standard, à laquelle on rajoute l'attribut “media” et une
  condition spécifique. Cette option n'est pas disponible en XHTML. N'oubliez
  pas que le dernier CSS chargé prend le pas sur les précédents. Pensez donc à
  toujours déclarer vos media-queries en dernier, sinon elles seront
  systématiquement écrasées par votre CSS “standard”

  ~~~
  <link rel="stylesheet" media="[condition]" href="[mon css spécial].css"/>
  ~~~
  {:.html}

* au sein même d'un fichier CSS, définir certains règles qui ne s'appliqueront
  que sous certaines conditions

  ~~~
  @media ([ma condition]) {
  	[mon sélecteur css] {
      		[mes propriétés]: [mes valeurs];
    	}
  }
  ~~~
  {:.css}

Une media query fonctionne de la manière suivante: la condition est évaluée et
retourne une valeur “vrai” ou “faux”. Si la valeur est vraie, le fichier CSS/ la
règle CSS (selon la méthode employée) est appliquée.

Chaque condition est formée à partir d'une ou plusieurs conditions de base. Les
conditions de base peuvent être combinées ensemble pour former des conditions
complexes en utilisant les opérateurs logiques:

* “and” (ET): les deux conditions de base doivent être vraies pour que la
  condition soit vraie
* “,” (OU): au moins l'un des conditions de base doit être vraie pour que la
  condition soit vraie
* “not” (NON): la condition de base qui suit le not doit être fausse pour que la
  condition soit vraie

Exemple : la règle media="(min-width: 500px) and (min-height: 800px)" 
Nous nous limiterons aux média queries sur la max-width 

<div class="exercise">
 1. Ouvrez maintenant votre site de Chuck Norris avec Chrome en choisissant un téléphone (genre Galaxy S3) pour la visualisation). 
 Constatez que le site s'affiche en tout petit. 
 1. Allez sur le site  Bootstrap .... constater les points de ruptures 

</div>


### Les points de ruptures.

Afin d'organiser notre media queries, on utilise en général 3 à 4 valeurs de largeur d'écrans, par exemple : 
 
 `480px` (SmartHpone)
 `768px` (Tablette)
 `992px` (écran d'ordinateur "Standard")
 `1200px` (écran d'ordinateur "Large")

<div class="exercise">
 1. en dessous de 768px ne plus afficher la table de comparaison. (De toute façon s'il ne doit rester qu'un seul, ce sera Chuck Norris)
 1. supprimer les marges latérales en dessous de 768px
 1. en dessous de 480px faire en sorte qu"`<aside>` et `<article>` soit en ligne et plus en colonne
 1. optionnel sur une smartwatch (width `168px`), n'affichez que les citations de Chuck Norris contenues dans le `<aside>`.
</div>


### Un bouton burger (AKA la blague du burger au menu)


* menu burger qui arrive de la gauche, par dessus la page, et qui grise le reste
de la page ? Le grisé avec une couche noire et de la transparence

Note :

* le hover sur les sous-menus n'a pas de sens sur téléphone portable. Il faudra
  gérer le clic avec du JS. Et dire, la bonne solution est ... et vous la verrer
  en 2ème année.



## Une solution plus pratique que les media queries 

### Le classique 12 colonnes.


### Responsive images 

* image responsive avec srcset ?
