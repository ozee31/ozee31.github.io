---
layout:     post
title:      "[Electron] - Electron + VueJS = electron-vue"
subtitle:   "Dans mon précédent article, je vous ai parlé d'Electron pour créer une application native en HTML/JS. Aujourd'hui je vais vous montrer comment il est simple de créer une application Electron grâce au framework JS VueJS."
date:       2016-10-13 08:00:00
comments: true
header-img: "img/electron.png"
hearder-bg-color: "rgba(43, 46, 59, 0.9)"
---

# Electron + VueJS = electron-vue

Dans mon précédent article, je vous ai parlé d'Electron pour créer une application native en HTML/JS. Aujourd'hui je vais vous montrer comment il est simple de créer une application Electron grâce au framework JS VueJS.

## La librairie

Nul besoin d'installer Vuejs et Electron séparément, une librairie se charge de tout : [electron-vue](https://www.gitbook.com/book/simulatedgreg/electron-vue/details).

Elle est bien sûr installable via `npm` mais avant ça il y a des prérequis (et je me suis bien pris la tête).

## Prérequis

Comme je l'ai dis précédemment, je me suis bien pris la tête pour réussir l'installation de cette librairie (et la documentation n'est pas très bavarde à ce sujet) mais comme je suis gentil je vais vous montrer comment faire pas à pas.

### Python

Donc la première chose à avoir sur son PC est [Python](https://www.python.org/downloads/).

Attention la version à télécharger, à l'heure où je parle (ou plutôt à l'heure où j'écris ces lignes), est la branche **2.x** (la branche 3.x n'étant pas compatible). Rassurez-vous les version 2.x et 3.x peuvent fonctionner parallèlement.

### Windows-Build-Tools

C'est sur cette partie que j'ai eu le plus de mal, en effet, ne connaissant pas cette librairie j'ai d'abord essayer d'installer ~~l'usine à gaz~~ Visual Studio afin de posséder le compilateur C++ nécessaire à l'installation de la librairie.

Mais grâce à cette simple librairie, tout parait (est) beaucoup plus simple. J'ai tout de même dû upgrader npm manuellement.

#### npm-windows-upgrade

Si vous êtes sur Linux ou Mac passez votre chemin ^^.

La première chose à faire est d'ouvrir **PowerShell** en mode **administrateur** et de lancer la commande :

{% highlight shell %}
Set-ExecutionPolicy Unrestricted -Scope CurrentUser -Force
{% endhighlight %}

Ensuite toujours en mode **administrateur** (mais pas forcément sur PowerShell) vous faites ceci

{% highlight shell %}
npm install --global --production npm-windows-upgrade
npm-windows-upgrade
{% endhighlight %}

et installez la dernière version de `NPM`

#### Windows Builds Tools

Vous pouvez installer `windows-build-tools` globalement

{% highlight shell %}
npm install --global windows-build-tools
{% endhighlight %}

## Installation

Maintenant on peut démarrer un projet electron-vue grâce à `vue-cli`

Si vue-cli n'est pas installé, installez le globalement :

{% highlight shell %}
npm install -g vue-cli
{% endhighlight %}

Ensuite on initialise et on installe les dépendances :

{% highlight shell %}
vue init simulatedgreg/electron-vue my-project

cd my-project
npm install
npm run dev
{% endhighlight %}

## Développement

Le gros point fort de cette librairie, est que, comme sur un projet VueJS classique, le Hot reload fonctionne.

Alors lancez au plus vite votre serveur de dev :

{% highlight shell %}
npm run dev
{% endhighlight %}

Pour la suite, si vous connaissez VueJS, vous ne serez pas perdu.

## Vous n'avez pas réussi à installer electron-vue ?

Si ce tuto n'a pas fonctionné chez-vous, laissez moi un commentaire avec le plus d'informations possible (SE, version de NPM...) car vu toutes les manipulations que j'ai dû effectuer avant d'arriver au résultat, il est possible que j'ai oublié 2 ou 3 détails.