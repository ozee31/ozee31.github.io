---
layout:     post
title:      "[vuejs] Méthodes publiques"
subtitle:   "Durant le processus de développement d'une application Vuejs, vous pouvez avoir besoin de faire communiquer un module externe avec votre application. Voici comment faire."
date:       2016-11-29 08:00:00
comments: true
header-img: "img/vuejs.png"
hearder-bg-color: "rgba(66, 185, 131, 0.9)"
---

# Vuejs - Méthodes publiques

Durant le processus de développement d'une application Vuejs, vous pouvez avoir besoin de faire communiquer un module externe avec votre application. Voici comment faire.

## Prérequis

Pour ce tuto j'ai utilisé la version **2.1.3** de vuejs

## But du tutoriel

Réussir à communiquer avec vuejs via le scope `window`. Nous allons ici juste donner la possibilité de modifier un texte dynamiquement.

## Le commencement

Un petit fichier `index.html` tout joli fera l'affaire

{% highlight html %}
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
</head>

<body>
  <div id="app">
    <p>{{ message }}</p>
  </div>

  <script src="app.js"></script>
</body>
</html>
{% endhighlight %}

Et un fichier `app.js`

{% highlight js %}
new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!'
  }
})
{% endhighlight %}

Par défaut on voit bien qu'il nous est impossible de communiquer depuis le scope `window` avec l'instance `Vue`.

## Première solution

La première solution qui nous viens à l'esprit est de passer notre instance de vue dans le scope global comme ceci :

{% highlight js %}
window.Vue = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!'
  }
})

// On peut maintenant communiquer n'importe quand avec notre instance vue comme ceci
window.Vue.$data.message = 'plop'
{% endhighlight %}

Cela à le mérite de fonctionner, mais...

### Ne pas faire ça !!!

Le gros danger ici est qu'un utilisateur lambda pourra accéder entièrement à votre application et tout modifier (y compris le store).

## La solution propre

La solution que j'ai retenu est de n'ouvrir qu'une seule méthode de notre instance qui nous permettra de communiquer de manière sécurisé avec l'extérieur.

{% highlight js %}
var vue = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    api: function (action, value) {
      switch (action) {
        case 'changeMessage':
          this.message = value
          break
      }
    }
  }
})

// on met la méthode api de notre instance Vuejs dans le scope window
window.api = vue.api

// on appelle notre api comme ceci
window.api('changeMessage', 'plip')
{% endhighlight %}

