---
layout:     post
title:      "[php] CakePHP 3.3 : Activer le CrossDomain (CORS) en utilisant le middleware"
subtitle:   "Dans un précédent article, je vous avais montré comment activer le CrossDomain sur votre application en utilisant les filtres du Dispatcher. Cependant depuis le version 3.3 de CakePHP cette technique est obsolète et doit être remplacée par le Middleware. Voici comment faire."
date:       2016-11-09 08:00:00
comments: true
header-img: "img/php.png"
hearder-bg-color: "#6082BB"
---

# CakePhp 3.3 : Activer le CrossDomain (CORS) en utilisant le middleware

Dans un [précédent article](http://www.flavienbeninca.fr/2016/07/12/cakephp-3-crosscomain-cors.html), je vous avais montré comment activer le CrossDomain sur votre application en utilisant les filtres du Dispatcher.

Cependant depuis le version 3.3 de CakePHP cette technique est obsolète et doit être remplacée par le Middleware.

Mais comme je suis généreux je vous ai concocté un petit plugin qui fera tout le boulot à votre place ~~ou preques~~.

## Installation

Rien de bien compliqué, un petit coup de composer et le tour est joué.

{% highlight shell %}
composer require ozee31/cakephp-cors
{% endhighlight %}

La seule condition est d'avoir CakePHP dans sa version `> 3.3.0`

## Activation

Là aussi, attention c'est très très difficile, il suffit de l'activer dans le fichier `bootstrap.php`

{% highlight php %}
<?php
Plugin::load('Cors', ['bootstrap' => true, 'routes' => false]);
{% endhighlight %}

## Magie, magie ~~et vos idée...~~

Et voilà les requêtes ajax CrossDomain fonctionnent, elle est pas belle la vie?

> Quoi c'est tout? C'est paramétrable? Ça fonctionne comment?

Où là doucement, je v'ai tout vous expliquer.

## Secret de fabrication

Comme vous vous en doutez, le plugin utilise le [Middleware](http://book.cakephp.org/3.0/fr/controllers/middleware.html) afin d’analyser la requête et donner la réponse adéquate au navigateur.

### Etapes

- Le navigateur envoi une requête `OPTIONS` au serveur
- Le plugin analyse les informations et renvoi les bons entêtes au navigateur si l'origine est autorisée (on verra ça dans le chapitre configuration)
- Si la réponse est correcte la navigateur peut envoyer la bonne requête au serveur (GET, POST...) qui sera traitée normalement

Pour des informations plus détaillées sur le CrossDomain je vous invite à [lire cet article](https://developer.mozilla.org/fr/docs/HTTP/Access_control_CORS).

### Comment le Middleware traite t'il la requête OPTIONS ?

#### Origin

Dans un premier temps, le middleware va valider que l'appelant est autorisé à effectuer l'appel (par défaut le plugin autorise tout le monde).

#### Headers

Ensuite le plugin analyse les entêtes, si un seul n'est pas autorisé la requête est refusée. (Par défaut le plugin autorise tous les entêtes).

#### Méthodes

Là encore le plugin autorise toutes les méthodes HTTP possible (GET, POST, PUT, DELETE)

#### Cache

Par défaut le plugin met en cache la réponse pendant un jour (le navigateur n'aura pas besoin de relancer une requête OPTION qui a déjà été envoyée)

## Configuration

Bien sûr il est possible de surcharger la configuration par défaut, tout se passe dans votre fichier `app.php`, il suffit d'ajouter la clé `Cors` qui contiendra toutes les options désirées.

Dans l'exemple ci-dessous nous souhaiterons autoriser les requêtes crossDomain de google, leboncoin et mozilla, nous n'autoriserons que le header authorization, les methodes POST et GET et désactiverons le cache.

{% highlight php %}
<?php
'Cors' => [
  'AllowOrigin' => ['http://google.fr', 'http://leboncoin.fr', 'http://mozilla.com'],
  'AllowMethods' => ['GET', 'POST'],
  'AllowHeaders' => ['authorization'],
  'MaxAge' => false,
]
{% endhighlight %}

Pour plus de détails sur la configuration [c'est ici](https://github.com/ozee31/cakephp-cors)

