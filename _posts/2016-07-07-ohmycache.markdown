---
layout:     post
title:      "[js] OhMyCache"
subtitle:   "OhMyCache est une librairie basée sur les api LocalStorage et SessionStorage qui vous permet de stocker facilement sur le navigateur du client n'importe quel type de données (string, int, array, objets...)"
date:       2016-07-07 08:00:00
comments: true
header-img: "img/js.png"
hearder-bg-color: "#323330"
---

# Nouveau projet stable : OhMyCache

## OhMyCache ?

OhMyCache est une librairie basée sur les API [LocalStorage](https://developer.mozilla.org/fr/docs/Web/API/Window/localStorage) et [SessionStorage](https://developer.mozilla.org/fr/docs/Web/API/Window/sessionStorage) qui vous permet de stocker facilement sur le navigateur du client n'importe quel type de données (string, int, array, objets...).

De plus il est possible d'enregistrer des données temporaire ou encore en lecture seule.

## Techniquement c'est comment ?

J'ai créé le projet grâce à [webpack](https://webpack.github.io/), on a donc du ECMAScript 2015, du ESLint tout ça pour finir avec une librairie [UMD](https://github.com/umdjs/umd).

La librairie n'utilise aucune dépendance externe, elle est compatible avec les navigateurs possédant les API LocalStorage et SessionStorage (soit IE 8+, Chrome, Firefox...).

Enfin la librairie est testé grâce à [Jasmine](http://jasmine.github.io/).

## Je la trouve où cette librairie ?

Les sources sont sur [github](https://github.com/ozee31/ohmycache/) et le projet peut être installé avec `npm` ou `bower`.

La documentation est, elle, accessible [ici](http://www.flavienbeninca.fr/ohmycache/).

## Une petite démo ?

Bien sûr, même plusieurs vu qu'on peut importer la librairie avec `import`, `require` ou à l'ancienne via la balise `<script>`.

### ECMAScipt 2015

L'avantage de cette technique est que vous pouvez importer seulement la classe dont vous avez besoin. En effet si vous n'utilisez que le `LocalStorage` vous allez importer la classe `Local` et non la classe `Session`.

{% highlight js %}
import {Local, Session} from 'OhMyCache'

// Je crée un nouvel item dans le cache Local
Local.set('prenom', 'Jean')

// Je crée un nouvel item dans le cache de Session
Session.set('prenom', 'Alain')

// Maintenant on va lire les données
Local.get('prenom') // Jean
Session.get('prenom') // Alain
{% endhighlight %}

### Require

La même chose avec `require()`

{% highlight js %}
var OhMyCache = require('OhMyCache')

// Je crée un nouvel item dans le cache Local
OhMyCache.Local.set('prenom', 'Jean')

// Je crée un nouvel item dans le cache de Session
OhMyCache.Session.set('prenom', 'Alain')

// Maintenant on va lire les données
OhMyCache.Local.get('prenom') // Jean
OhMyCache.Session.get('prenom') // Alain
{% endhighlight %}

### Script

Et enfin à l'ancienne

{% highlight html %}
<html>
  <head>
    <!-- si installé via npm -->
    <script src="node_modules/ohmycache/dist/bundle.js"></script>

    <!-- si installé via bower -->
    <script src="bower_components//ohmycache/dist/bundle.js"></script>

    <!-- si installé à l'arrache -->
    <script src="lib-path/ohmycache/dist/bundle.js"></script>
    <script>
    // Je crée un nouvel item dans le cache Local
    OhMyCache.Local.set('prenom', 'Jean')

    // Je crée un nouvel item dans le cache de Session
    OhMyCache.Session.set('prenom', 'Alain')

    // Maintenant on va lire les données
    OhMyCache.Local.get('prenom') // Jean
    OhMyCache.Session.get('prenom') // Alain
    </script>
  </head>
</html>
{% endhighlight %}

## Et c'est tout ?

Je vous rassure on peut faire beaucoup plus.

{% highlight js %}
// On peut stocker des objets
OhMyCache.Local.set('user', {prenom: 'Jean', nom: 'Bon'})

// On peut faire expirer des données
OhMyCache.Local.set('token', 'token_securise', {expire: 3600}) // Notre token expirera dans 1 heure

// On peut aussi insérer des données en lecture seule
OhMyCache.Local.set('equipe_prefere', 'OM', {readonly: true}) // return true
OhMyCache.Local.set('equipe_prefere', 'PSG') // return false (en même temps ça devrait être interdit...)
OhMyCache.Local.get('equipe_prefere') // return 'OM'
OhMyCache.Local.remove('equipe_prefere') // return false
OhMyCache.Local.get('equipe_prefere') // return 'OM'
{% endhighlight %}

## What else

Le projet n'en est qu'à son début mais fonctionne bien (je l'utilise sur divers projets) mais je serais ravis d'avoir vos retours et idées d'amélioration.
J'ai vraiment voulu faire une librairie simple d'utilisation et la plus respectueuse des bonnes pratiques possible.