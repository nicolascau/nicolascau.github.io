---
layout: post
title:  "Estimer ses coûts de licence(s) Oracle, c’est compliqué !"
date:   2019-01-21 14:00:00 +0100
categories: [Chiffrage]
tags: [Oracle, Licence]
description: "Récemment, j’ai dû m’attaquer à l’exercice de chiffrer le coût en licence d’une base de données Oracle. J’ai remarqué que l’exercice était loin d’être aisé. Je me dis donc qu’un billet pourrait permettre d’y voir un peu plus clair pour tout le monde :"
description_questions: ["Quel est le mécanisme d’estimation des coûts en licence dans le cadre Oracle ?", "Quelles sont les données minimales nécessaires à l’estimation d’un coût de licence ?"]
---
{% include post_header.html %}

Bonne lecture !

# Introduction
Tout d’abord, je préfère ne pas vous promettre monts et merveilles. A la fin de cet article, vous ne saurez pas parfaitement estimer le coût en licence. Vous allez me répondre : Mais alors, il ne sert à rien ton article !

Oui et non, je m’explique. Cet article a plutôt vocation à vous donner des guides et une meilleure compréhension du sujet avant de contacter Oracle. Vous serez alors mieux armé pour les « affronter » (j’exagère sur les termes, quoique).

> Warning : De plus, je ne suis pas un expert Oracle. Je ne remplacerais pas la [documentation officielle](https://docs.oracle.com/cd/B28359_01/license.111/b28287/toc.htm) sur le sujet et un coup de fil au service commercial d’Oracle.


# Prérequis
Avant de débuter, je dois vous préciser qu’à l’heure de sortie de ce billet, je travaille dans une ESN majeure. Ceci apporte, bien évidemment des avantages :
* Partenariat avec Oracle ce qui a des implications directes sur le coût
* Point(s) de contact dédié(s) côté Oracle

Si vous travaillez dans une ESN importante, vous disposerez probablement d’une équivalence. Dans une autre structure, ce sera surement moins le cas.

Je ne m’attarde pas non plus sur la partie dimensionnement de votre machine contenant Oracle. Je pars donc du principe que vous savez quoi demander en terme matériel.

 
# 1. 1ère étape : Les différentes approches de coût
Une licence Oracle peut s’estimer selon deux approches :
* `Named User Plus` : via le nombre d’utilisateurs
* `Processor` : via le nombre de cœur de sa machine

Vous trouverez ci-dessous un détail vulgarisé de ces deux approches.


## 1.1. L’approche Named User Plus (NUP)
La métrique `Named User Plus` vous permet d’identifier le coût en licence dans une approche de nombre d’utilisateurs. 

Je ne vais pas m’étendre trop sur cette approche puisque je ne l’ai pas mise à l‘épreuve. Cela étant, il faut savoir qu’elle implique de connaître :
* Le nombre d’utilisateur allant se connecter à l’application (et non le nombre de pool de connexion à la base de données)
* Le nombre d’interaction de systèmes automatisés tels que des bus de données, des injecteurs de données, des batchs automatisés, etc.

> Warning : Comme on peut le voir dans les exemples que je cite, le terme « Named User Plus » est trompeur. En effet, il n’est pas compté que les utilisateurs d’un système. 

Dans les gros systèmes, ce nombre de NUP peut monter très rapidement. Il est préférable de réserver cette approche à un nombre limité d’utilisateur. Je vous conseille de vous méfier de cette approche si vous souhaitez utiliser votre plateforme pour réaliser des tests de performance également.

## 1.2. L’approche Processor
La métrique `Processor` vous permet d’identifier le coût en licence dans une approche nombre de processeurs. Dans ce cas de figure, il n’y a donc pas de limite au nombre d’utilisateur.
Il y a plusieurs règles à garder en tête :
* Si vous voulez intégrer Oracle dans un environnement virtualisé, il faut savoir que le coût de licence s’appuie sur le nombre de cœurs physiques de votre serveur (et non le nombre de CPU émulé par votre outil de virtualisation sur votre machine virtuelle).

* Pire, si vous utilisez une même instance VMWare adressant plusieurs serveurs physiques, Oracle comptabilisera l’ensemble des serveurs physiques pour estimer ses coûts de licence (la note risque d’être salée !). 

> Warning : Au vu de ce constat, il est préférable de ne pas intégrer Oracle dans un environnement virtualisé contenant d’autres machines virtualisées (dédiées elles à d’autres taches). Je conseille donc de réserver un serveur physique dédié à votre utilisation d’Oracle. Vous pouvez alors le dimensionner au vu de vos seuls besoins Oracle. Cette approche vous permet de contrôler finement l’empreinte physique et donc d’affiner votre coût de licence.

> Warning : Je suis volontairement réducteur sur ce point. Oracle reconnait certaines technologies de partitionnement d’un serveur. Vous trouverez plus d’information au [chapitre 7](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=7&ved=2ahUKEwippfWh0_7fAhXJzKQKHZXsBeQQFjAGegQIBRAC&url=https%3A%2F%2Fwww.aspera.com%2Ffile%2F33946%2Flivre_blanc_10_choses_a_savoir_audit_oracle%25253Fpage%25253D2214%252526L%25253D2&usg=AOvVaw1p5_oyiEwbDKbHfPSb8kzX) du document que je cite en fin de billet.

* Le modèle de serveur a également son importance. En fonction du type de cœur que détient votre serveur physique, Oracle applique un ratio dans ses calculs de coût. Vous trouverez plus de détail sur ce point dans le chapitre suivant.

Ça y est, vous avez désormais plusieurs éléments de compréhension pour mieux estimer vos coûts de licence Oracle.

Passons maintenant à l’étape suivante : demander un devis à Oracle.


# 2. 2ème étape : Demander un devis
Maintenant que nous connaissons les quelques leviers généraux, nous pouvons effectuer une demande de devis à Oracle.


## 2.1. Les règles à savoir pour contacter Oracle
Lorsque vous contactez Oracle, sachez qu’ils répartissent leur prestation selon le client à adresser. En fonction du client final (ce n’est parfois pas vous-même mais l’entreprise pour laquelle vous réalisez le projet), Oracle vous mettra en contact avec un commercial ou un autre. Il faut donc bien savoir quelle est l’entreprise cliente destinée à disposer de la base de données Oracle.

Ensuite, n’hésitez pas à exposer votre cas d’utilisation à Oracle. Vous pourrez ainsi vous appuyer sur leurs conseils pour adapter votre solution technique. Ne partez pas du principe que vous savez précisément ce que vous voulez. Prenez plutôt l’explication des mécanismes de licence Oracle comme un bonus d’explication afin de pouvoir demander des justifications de coût à Oracle dans son devis.


## 2.2. Les métriques nécessaires à Oracle
Pour effectuer un devis, Oracle vous demandera les données suivantes :
* Si vous effectuez une approche NUP ou Processor :
    * Dans le cas NUP
        * Edition de la base de données
        * Options supplémentaires souhaitées
        * Nombre d’utilisateurs global (cf. explication dans la 1ère étape)
        * Durée de licence
    * Dans le cas Processor
        * Edition de la base de données
        * Options supplémentaires souhaitées
        * Typologie du serveur : modèle de processeur et nombre de cœur physique
        * Durée de licence

Je détaillerais par la suite le cas « Processor » uniquement.


### 2.2.1. Métrique N°1 : Edition de la base de données
Plusieurs éditions de base de données Oracle sont disponibles et diffèrent fortement vis-à-vis des fonctionnalités, tarifs et limitations engendrées. 

Je vous citerais ici les 4 versions les plus courantes (il en existe d’autres) :
* `Oracle Database Enterprise Edition (EE)` : version la plus complète et la plus coûteuse. Grâce à cette version, il n’y a pas de limitation matérielle ou utilisateurs. Il est possible d’accéder au paramétrage de la base de données pour des besoins de tuning (entre autres). Il est également possible d’y ajouter des options à acheter en complément du coût de la licence.

> Warning : Vous pourrez retrouver l’ensemble des options disponibles décrites au sein de [cette documentation](https://docs.oracle.com/en/database/oracle/oracle-database/18/dblic/Licensing-Information.html#GUID-0F9EB85D-4610-4EDF-89C2-4916A0E7AC87).

> Warning : Cette version est intéressante dans un cadre professionnel où il y a des besoins importants en ressource et/ou des besoins de tuning.

* `Oracle Database Standard Edition 2 (SE2)` : version plus limitée que la version EE. Il n’existe pas d’option disponible. Il n’est pas possible d’accéder au paramétrage de la base de données (tout du moins, seulement de façon limitée). De plus, cette version est limitée à 2 sockets et une utilisation à 16 threads maximum par CPU.
> Warning : Cette version est intéressante dans un cadre professionnel standard où les besoins en ressources sont maîtrisés et conforme aux limitations évoquées ci-dessus.

* `Oracle Database Personal Edition (PE)` : version à un seul utilisateur qui permet une compatibilité avec les versions EE et SE2. Celle-ci inclue plusieurs composants de la version EE.
> Warning : Cette version est intéressante dans un cadre de développement et de tests mono-utilisateur.

* `Oracle Database Express Edition (XE)` : version gratuite permettant d’utiliser Oracle Database pour ses besoins personnels. Celle-ci dispose de fortes limitations et est davantage à réserver à un cadre non professionnel.
> Warning : Cette version est à limiter à un cadre non professionnel (elle n’est pas adaptée à une utilisation en production par exemple). Plus de détail disponible [ici](https://www.oracle.com/database/technologies/appdev/xe.html).

Généralement, dans un cadre d’application en production, vous hésiterez entre la version EE et la version SE2. Et au vu des écarts de tarif (conséquents), vous vous poserez la question : 
* Les limitations de la version SE2 sont-elles compatibles avec l’utilisation que je prévoie de ma base de données ?


### 2.2.2. Métrique N°2 : Options supplémentaires souhaitées
Je ne vais pas être très verbeux sur le sujet. La documentation Oracle vous en dira davantage.
* Sachez que chaque édition d’Oracle permet de disposer d’une option de manière plus ou moins complète. Ce référencement est disponible [ici](https://docs.oracle.com/en/database/oracle/oracle-database/18/dblic/Licensing-Information.html#GUID-0F9EB85D-4610-4EDF-89C2-4916A0E7AC87).
* Sachez que pour certaines éditions d’Oracle (la EE entre autres), il est possible d’y ajouter des fonctionnalités supplémentaires. La liste des fonctionnalités supplémentaires est disponible [ici](https://docs.oracle.com/en/database/oracle/oracle-database/18/dblic/Licensing-Information.html#GUID-AB56CEE3-955E-4E56-8B44-6075E889C283).
* Sachez que pour certaines d’éditions d’Oracle (la EE entre autres), il est possible de souscrire à des packs comprenant une liste de fonctionnalités supplémentaires. Ce descriptif est disponible [ici]( https://docs.oracle.com/en/database/oracle/oracle-database/18/dblic/Licensing-Information.html#GUID-68A4128C-4F52-4441-8BC0-A66F5B3EEC35).

> Warning : Oracle fournit également une procédure pour vérifier les options activées sur votre base de données. Ceci nécessite l’usage d’un script SQL. L’ensemble de la procédure est disponible [ici]( https://docs.oracle.com/en/database/oracle/oracle-database/18/dblic/Licensing-Information.html#GUID-C3042D9A-5596-41A3-A08A-4581FED7634F).

Il peut résider une vraie difficulté pour savoir ce qui est activé ou non de base au sein de vos options de base de données. Cela est accentué par le fait que plusieurs fonctionnalités sont disponibles mais non activées dans une version SE2. Il suffit alors simplement de les activer. 

Il est donc intéressant d’appliquer cette vérification car toute activation d’une fonctionnalité non prévue dans l’édition que vous avez causera une requalification automatique en EE (et donc Oracle sera en mesure de vous demander un réalignement financier).


### 2.2.3. Métrique N°3 : Typologie du serveur
Dans une approche `Processor`, Oracle part du principe que l’utilisation de son produit Oracle diffèrera d’un serveur à un autre en fonction de sa puissance. Un coefficient est donc appliqué selon le serveur choisit pour héberger votre Oracle : `le Core Factor`. Ceci signifie qu’il vous faut préciser le modèle de processeur que vous souhaitez utiliser.
> Warning : Vous pouvez retrouver une liste (qui date de 2018) des processeurs et du coefficient appliqué [ici](http://www.oracle.com/us/corporate/contracts/processor-core-factor-table-070634.pdf).

A partir du processeur utilisé et selon l’édition choisit, il faut :
* Soit prendre en compte le nombre de processeur
* Soit prendre en compte le nombre de cœur

Ce choix est explicité au sein de la documentation ci-dessous :
 
![Documentation officielle Oracle sur la licence Processeur](/assets/2019-01-21-estimer-ses-couts-de-licences-oracle-c-est-complique/01.png)
 
![Documentation officielle Oracle sur les conditions liées aux éditions standards](/assets/2019-01-21-estimer-ses-couts-de-licences-oracle-c-est-complique/02.png)

Dans le cas d’une édition SE, le calcul de licence se base sur le nombre de sockets disponibles. On peut partir du postula qu’un processeur équivaut à un socket. 
> Warning : C’est donc le nombre de processeur qui sera nécessaire à Oracle pour estimer le coût.

Dans le cas d’une édition EE, le calcul de licence se base sur le nombre de cœurs disponibles. Il sera appliqué le `Core Factor` à ce nombre de cœurs. 
> Warning : C’est donc le nombre de cœurs qui sera nécessaire à Oracle pour estimer le coût.


### 2.2.4. Métrique N°4 : Durée de licence
Oracle ne vous précisera peut-être pas ceci en premier lieu. Néanmoins, sachez qu’une licence Oracle n’est pas nécessairement perpétuelle. Si vous ne le précisez pas à la demande, sachez qu’Oracle prendra surement l’hypothèse d’une licence perpétuelle.
Oracle propose donc 2 types de licences :
* La `licence perpétuelle` : comme son nom l’indique, celle-ci ne dispose pas de fin de vie.
* La `licence à terme` : celle-ci présente un coût inférieur à la licence perpétuelle mais n’est valable que pour un nombre d’année données. Ce type de licence est possible pour des durées de 1 à 5 ans.

> Warning : Retour d’Oracle : `Le prix affiché pour une licence à terme est basé sur un pourcentage spécifique du prix de la licence perpétuelle. Les licences de durées annuelles sont disponibles de 1 à 5 ans : 1 an - 20% de la liste ; 2 ans - 35% de la liste ; 3 ans - 50% de la liste ; 4 ans 60% de la liste ; 5 ans 70% de la liste.`

> Warning : Attention au passage de licence à terme à licence perpétuelle. Oracle ne vous fera pas payer la différence mais vous comptera un coût de licence perpétuelle à 100%.


# 3. 3ème étape : Réception du devis
Vous avez effectué votre demande de devis, Oracle vous transmet alors une proposition commerciale. Celle-ci précise généralement les informations suivantes :
* 1 ligne dédiée au coût de la licence proprement dites : celle-ci y précise l’édition de la base de données (cf. 3.3) et la durée de licence choisie (cf. 3.6).
* 1 ligne dédiée au coût de support et mise à jour logiciel : celle-ci, dans un cas nominal, est fournie pour une durée d’1 an.
Dans le cadre de vos appels d’offre, vous souhaitez potentiellement vous projeter sur plusieurs années et donc estimer le coût que cela engendrerait. 
* Le coût de licence n’est qu’à estimer une seule fois car son coût est valable selon la durée choisie (donc potentiellement de façon perpétuel).
* Le coût de support et mise à jour logiciel suit une règle d’estimation au fil des années. Selon des retours d’Oracle, la règle actuelle est la suivante :

> Warning : Retour d’Oracle : `Il faut compter 22 % du prix d’achat pour une licence pour la première année mais Oracle applique ensuite un IAR (inflation rate) qui est équivalent à environ 3 ou 4 % d’augmentation par an pour chaque année de support complémentaire.`

 
# Conclusion
J’espère vous avoir éclairé sur la logique de licences concernant la partie base de données Oracle. Ce billet n’a pas la prétention à donner une vision exhaustive du sujet mais plutôt à vous donner un vernis. Grâce à ces quelques éléments, vous devriez mieux savoir :
* Quelle édition répondra à vos besoins ?
* Quel matériel associé à vos besoins pour adapter vos coûts de licence de façon réaliste ?


Pour écrire ce billet, je me suis appuyé sur mon expérience personnelle mais également sur la lecture des documents suivants :
* [Cet article](https://www.softwareone.com/en/blog/licensing-oracle-by-processor-or-named-user-plus-which-metric-is-right-for-you) vous donnera un avant-goût du sujet en survolant quelques problématiques
* Et je recommande spécialement la lecture de [ce document](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=7&ved=2ahUKEwippfWh0_7fAhXJzKQKHZXsBeQQFjAGegQIBRAC&url=https%3A%2F%2Fwww.aspera.com%2Ffile%2F33946%2Flivre_blanc_10_choses_a_savoir_audit_oracle%25253Fpage%25253D2214%252526L%25253D2&usg=AOvVaw1p5_oyiEwbDKbHfPSb8kzX) qui clarifie de façon plus exhaustive les règles s’appliquant aux licences Oracle (même si celui-ci date de 2016).

Plusieurs éléments sont volontairement écartés de ce billet. Je vous précise ici les critères que j’ai eu à prendre en compte. Néanmoins, il faut savoir qu’il existe d’autres variables d’ajustement :
* La notion de licence perpétuelle ou temporaire ;
* Les minimums de licence NUP à utiliser selon l’édition choisie ;
* Les solutions de partitioning au niveau physique ou logique possibles ;
* D’autres éditions d’Oracle.

Qui sait, peut être y’aura-t-il un complément à ce billet !
