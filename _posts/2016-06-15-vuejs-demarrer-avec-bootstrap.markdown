---
layout:     post
title:      "[vuejs] Démarrer avec bootstrap"
subtitle:   "Dans ce tutoriel, nous allons voir comment démarrer un projet avec Bootstrap (en mode SASS) sur le framework Vuejs"
date:       2016-06-15 08:00:00
comments: true
header-img: "img/vuejs.png"
hearder-bg-color: "rgba(66, 185, 131, 0.9)"
---

# Vuejs - Démarrer avec bootstrap

Dans ce tutoriel, nous allons voir comment démarrer un projet avec [Bootstrap](http://getbootstrap.com/) (en mode SASS) sur le framework [Vuejs](https://vuejs.org/)

## Prérequis
Pour suivre ce tuto vous devez connaitre un minimum le framework [Vuejs](https://vuejs.org/)

## Environnement
Voici ma configuration pour cette démo

- Une machine Windows 10 (ne me fouettez pas SVP)
- NPM en version 5
- Vuejs en version 1.0.21 avec webpack
- Bootstrap-sass en version 3.3.6

## Création du projet
Je démarre toujours un projet Vuejs grâce à vue-cli afin d'installer Vuejs grâce à webpack.

Si ce n'est pas déjà fait vous devez l'installer globalement

{% highlight shell %}
npm install -g vue-cli
{% endhighlight %}

On peut ensuite créer le projet

{% highlight shell %}
vue init webpack tuto
{% endhighlight %}

Vous vous déplacez dans le dossier du projet et vous installez les dépendances de vuejs

{% highlight shell %}
cd tuto
npm install
{% endhighlight %}

Enfin on installe les dépendances propre à environnement à savoir : bootstrap, jquery et ce qu'il faut pour compiler le sass

{% highlight shell %}
npm install -S bootstrap-sass
npm install -S jquery@2
npm install sass-loader node-sass webpack --save-dev
{% endhighlight %}

## Préparation de la vue

Nous allons modifier le fichier de base `App.vue`afin d'intégrer un petit exemple de page utilisant bootstrap.

{% highlight html %}
// src/App.vue

<template>
  <div id="app">
    <div class="container">

      <!-- Static navbar -->
      <nav class="navbar navbar-default">
        <div class="container-fluid">
          <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
              <span class="sr-only">Toggle navigation</span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">Project name</a>
          </div>
          <div id="navbar" class="navbar-collapse collapse">
            <ul class="nav navbar-nav">
              <li class="active"><a href="#">Home</a></li>
              <li><a href="#">About</a></li>
              <li><a href="#">Contact</a></li>
              <li class="dropdown">
                <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Dropdown <span class="caret"></span></a>
                <ul class="dropdown-menu">
                  <li><a href="#">Action</a></li>
                  <li><a href="#">Another action</a></li>
                  <li><a href="#">Something else here</a></li>
                  <li role="separator" class="divider"></li>
                  <li class="dropdown-header">Nav header</li>
                  <li><a href="#">Separated link</a></li>
                  <li><a href="#">One more separated link</a></li>
                </ul>
              </li>
            </ul>
            <ul class="nav navbar-nav navbar-right">
              <li class="active"><a href="./">Default <span class="sr-only">(current)</span></a></li>
              <li><a href="../navbar-static-top/">Static top</a></li>
              <li><a href="../navbar-fixed-top/">Fixed top</a></li>
            </ul>
          </div><!--/.nav-collapse -->
        </div><!--/.container-fluid -->
      </nav>

      <!-- Main component for a primary marketing message or call to action -->
      <div class="jumbotron">
        <h1>Navbar example</h1>
        <p>This example is a quick exercise to illustrate how the default, static navbar and fixed to top navbar work. It includes the responsive CSS and HTML, so it also adapts to your viewport and device.</p>
        <p>
          <a class="btn btn-lg btn-primary" href="../../components/#navbar" role="button">View navbar docs »</a>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import Hello from './components/Hello'

export default {
  components: {
    Hello
  }
}
</script>
{% endhighlight %}

Maintenant si vous lancez le serveur de dev avec la commande `npm run dev` pour pouvez voir une belle page non stylé.

## Je veux du style

### Architecture

Dans le dossier `assets` je vous invite à créer des dossiers et fichiers vide comme ci-dessous

```
src/
---assets/
------scss/
---------design.scss
---------bootstrap/
------------bootstrap-custom.scss
------------variables.scss
```

### design.scss

C'est le fichier principal, c'est lui qui sera appelé par votre page `App.vue`

{% highlight scss %}
/* On importe bootstrap */
@import "./bootstrap/bootstrap-custom";

/* On fait ce qu'on veut après */
{% endhighlight %}

### variables.scss

Ce fichier permet de personnaliser bootstrap à votre guise, voici un exemple de surcharge du fichier par défaut

{% highlight scss %}
/**
 * Toute cette partie permet de surcharger les variables de bootstrap
 */

/* La je modifie la couleur principale */
$brand-primary: #5a3966;


/**
 * J'importe enfin le fichier de variables par défaut de bootstrap qui va permettre de définir les variables que je n'ai pas surchargées
 */
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/variables";
{% endhighlight %}

### bootstrap-custom.scss

Ce fichier va charger tous les modules de bootstrap

{% highlight scss %}
/* Core variables and mixins */
@import "./variables";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/mixins";

/* Reset and dependencies */
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/normalize";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/print";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/glyphicons";

/* Core CSS */
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/scaffolding";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/type";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/code";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/grid";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/tables";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/forms";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/buttons";

/* Components */
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/component-animations";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/dropdowns";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/button-groups";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/input-groups";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/navs";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/navbar";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/breadcrumbs";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/pagination";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/pager";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/labels";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/badges";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/jumbotron";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/thumbnails";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/alerts";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/progress-bars";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/media";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/list-group";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/panels";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/responsive-embed";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/wells";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/close";

/* Components JavaScript */
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/modals";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/tooltip";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/popovers";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/carousel";

/* Utility classes */
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/utilities";
@import "node_modules/bootstrap-sass/assets/stylesheets/bootstrap/responsive-utilities";
{% endhighlight %}

Libre à vous de surcharger ou de ne pas charger des modules en particulier. On voit qu'on a bien surcharger notre fichier `variable.scss`

### Allez c'est parti

Il nous reste plus qu'à charger notre fichier principal dans la vue

{% highlight html %}
// Code à rajouter dans le ficier "App.vue"

<style lang="scss">
  @import 'src/assets/scss/design.scss';
</style>
{% endhighlight %}

Et quand regarde le résultat......... **BOOM !!!**

    Module not found: Error: Cannot resolve 'file' or 'directory' ../fonts/bootstrap/glyphicons-halflings-regular.eot

En fait bootstrap ne parviens plus à charger ses fontes, il va juste falloir surcharger une petite variable

{% highlight sass %}
// Dans "variables.scss" surchargez cette variable (insérer la ligne avant le @import final, pas après)

$icon-font-path: "/node_modules/bootstrap-sass/assets/fonts/bootstrap/";
{% endhighlight %}

Maintenant on a une "belle" page mais si vous essayez de déplier le Dropdown cela ne fonctionne pas. C'est tout à fait normal car le javascript n'est pas chargé.

## Javascript

Pour charger la librairie js de bootstrap rien de plus simple

{% highlight js %}
// Code à rajouter dans votre fichier "src/main.js"

import jQuery from 'jquery'
window.jQuery = window.$ = jQuery
require('bootstrap-sass')
{% endhighlight %}

Et voilà vous avez maintenant un Bootstrap entièrement personnalisable pour votre application Vuejs