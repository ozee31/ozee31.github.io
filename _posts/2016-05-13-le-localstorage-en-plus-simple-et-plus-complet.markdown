---
layout:     post
title:      "[js] Le LocalStorage, en plus simple et plus complet"
subtitle:   "Cachejs est une librairie qui permet de stocker des données sur le PC du client en utilisant le LocalStorage ou le SessionStorage"
date:       2016-05-13 08:00:00
comments: true
header-img: "img/js.png"
hearder-bg-color: "#323330"
---

# Le LocalStorage, en plus simple et plus complet
Nous allons voir qu'avec la librairie Cachejs, il est relativement simple de stocker des données côté client en utilisant le LocalStorage du navigateur.

## Quésaco ?

On va commencer par une petite définition à base de wikipedia
> Le stockage web local ou stockage DOM (Document Object Model) est une technique d'enregistrement de données dans un navigateur web. Le stockage web local permet l'enregistrement persistant, comme avec les cookies mais avec une capacité bien plus grande, et sans avoir à rajouter de données dans l'entête de requête HTTP. Il existe deux types de stockage web local : le stockage local et le stockage de session, équivalant respectivement aux cookies persistants et aux cookies de session.

LocalStorage est donc une API qui est compatible avec la plupart des navigateurs (pour peux qu'on soit moderne), à savoir Chrome, Firefox, IE (à partir de la 8), Opera, Safari.

## En avant ~~Guingamp~~

Pour cette démonstration nous n'avons pas besoin de grand chose, on va créer une page HTML avec un script JS donc pas besoin de serveur Apache ou autre ça va être très simple.

Je vais juste utiliser `npm` pour installer la librairie mais libre à vous d'utiliser `bower` ou de télécharger les sources directement sur github.

D'ailleur en parlant de source voici le [repository](https://github.com/ozee31/cachejs)

## Installation et préparation


Pour cette démonstration je vais initialiser un projet npm

{% highlight shell %}
npm init
{% endhighlight %}

Puis j'installer la librairie (à ce jour la version 1.0)

{% highlight shell %}
npm install ozee-cachejs
{% endhighlight %}

Je vais créer ensuite un fichier `index.html`

{% highlight html %}
<!doctype html>

<html lang="fr">
<head>
  <meta charset="utf-8">
  <title>Cachejs demo</title>
</head>
<body>
  <div id="output">

  </div>

  <script src="node_modules/ozee-cachejs/dist/cache.min.js"></script>
  <script src="demo.js"></script>
</body>
</html>
{% endhighlight %}

Puis un fichier `demo.js` avec un petit bout de code qui va nous permettre de savoir si la lib est chargé

{% highlight js %}
(function() {
  var output = document.getElementById('output');

  if ( typeof Cachejs === 'undefined' ) {
      output.innerHTML = 'Lib non chargé';
      return false;
  }

  output.innerHTML = 'Lib chargé';
})();
{% endhighlight %}

On démarre notre fichier index.html dans notre navigateur favori et si tout se passe bien on devrait voir s'afficher

> Lib chargé

Si ce n'est pas le cas je vous invite à vérifier le chemin vers la livrairie.

## Présentation

Avant de commencer à utiliser la librairie, je pense q'une petite présentation s'impose.

### 1 = 2

> Tu te fous pas un peu de moi là ?

Je vous rassure, je suis meilleur que ça en mathématique. Tout ça pour vous dire que cette librairie est composé de 2 classes qui partagent exactement la même API.

- `Cachejs.Local` : utilise [le localStorage](https://developer.mozilla.org/fr/docs/Web/API/Window/localStorage)
- `Cachejs.Session` : utilise [le sessionStorage](https://developer.mozilla.org/fr/docs/Web/API/Window/sessionStorage)

> La propriété localStorage vous permet d'accéder à un objet local Storage. Le localStorage est similaire au sessionStorage. La seul différence : les données stockées dans le localStorage n'ont pas de délai d'expiration, les données stockées dans le sessionStorage sont nettoyées quand la session navigateur prend fin — donc quand on ferme le navigateur.

### Pourquoi ?

> Et elle m'apporte quoi cette librairie par rapport à l'API de base ?

Concrètement, par rapport à l'api standard vous pourrez :

- stocker des données complexe (array, object)
- faire expirer vos données au bout d'un certain temps
- stocker des données en lecture seule

### Méthodes man

Voici un bref aperçu des méthodes disponible :

- **set** : stocker une donnée
- **get** : récupérer une donnée stocké
- **remove** : supprimer une donnée
- **clear** : vider tout le cache
- **isExpired** : indique si la donnée a expiré
- **isReadonly** : indique si la donnée est en lecture seule

## Alors on ~~danse~~ commence

### Set et match
Nous allons stocker divers types de données (string, array, objet...) et les afficher ensuite dans notre balise `output`. Je vous invite donc à modifier le fichier `demo.js`

{% highlight js %}
// demo.js

(function() {
    var output = document.getElementById('output');

    if ( typeof Cachejs === 'undefined' ) {
        output.innerHTML = 'Lib non chargé';
        return false;
    }

    // On réinitialise le cache
    Cachejs.Local.clear();
    Cachejs.Session.clear();

    /**
     * Stockage des données
     */
        // stockage d'un string
        Cachejs.Local.set('monString', 'je suis un string');

        // stockage d'un booléen en Session
        Cachejs.Session.set('monBool', true);

        // stockage d'un array
        Cachejs.Local.set('monArray', [1,2,3]);

        // stockage d'un objet
        Cachejs.Local.set('monObjet', {prenom: 'Lary', nom: 'Gole'});


    /**
     * Récupération des données
     */
        output.innerHTML = '';

        // On affiche le cache local de la clé 'monString'
        output.innerHTML += 'monString = ' + Cachejs.Local.get('monString') + '<br>';

        // Si on affiche le cache local de la clé 'monBool' cela affiche 'null' car il a été enregistré en cache de session
        output.innerHTML += 'monBool (local) = ' + Cachejs.Local.get('monBool') + '<br>';

        // En revanche avec le cache session ça marche
        output.innerHTML += 'monBool (sessoin) = ' + Cachejs.Session.get('monBool') + '<br>';

        // On affiche notre array
        output.innerHTML += 'monArray = ' + Cachejs.Local.get('monArray') + '<br>';

        // Et pour finir notre objet
        output.innerHTML += 'monObjet = ' + JSON.stringify( Cachejs.Local.get('monObjet') ) + '<br>';
})();
{% endhighlight %}

Une fois la page rafraîchie vous devrez voir apparaître :

```
monString = je suis un string
monBool (local) = null
monBool (sessoin) = true
monArray = 1,2,3
monObjet = {"prenom":"Lary","nom":"Gole"}
```

### Aller plus loin

Si vous êtes toujours là (et je l'espère), on va comme le titre l'indique, aller plus loin, en utilisant l'expiration des données et la lecture seule.
On repart à zéro avec notre `demo.js`

{% highlight js %}
// demo.js

(function() {
    var output = document.getElementById('output');

    if ( typeof Cachejs === 'undefined' ) {
        output.innerHTML = 'Lib non chargé';
        return false;
    }

    // On réinitialise le cache
    Cachejs.Local.clear();
    Cachejs.Session.clear();
    output.innerHTML = '';

    /**
     * Lecture seule
     */
        Cachejs.Local.set('readOnly', 'Je suis en lecture seule', null, true);

        // Je ne peux donc pas modifié la donnée
        Cachejs.Local.set('readOnly', 'Je ne peux pas être modifié');
        output.innerHTML += 'readOnly = ' + Cachejs.Local.get('readOnly') + '<br>';

        // Je ne peux pas la supprimer non plus
        Cachejs.Local.remove('readOnly');
        output.innerHTML += 'readOnly = ' + Cachejs.Local.get('readOnly') + '<br>';

    /**
     * Expiration
     */
        Cachejs.Local.set('expiration', 'Je vais expirer dans 3 secondes', 3);

        // J'existe encore
        output.innerHTML += 'expiration = ' + Cachejs.Local.get('expiration') + '<br>';

        // Dans 4 secondes j'existe plus
        setTimeout(function(){ output.innerHTML += 'expiration = ' + Cachejs.Local.get('expiration') + '<br>' },4000);
})();
{% endhighlight %}

On recharge, on attend 4 secondes et le résultat sera :

```
    readOnly = Je suis en lecture seule
    readOnly = Je suis en lecture seule
    expiration = Je vais expirer dans 3 secondes
    expiration = null
```