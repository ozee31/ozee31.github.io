---
layout:     post
title:      "[vuejs] Une configuration différente pour chaque environnement"
subtitle:   "Lors du développement d’une application web basée sur VueJS, j’ai eu besoin d’appeler mon API via une url différente en fonction de mon environnement (local, preview et production). Ce tuto sous vuejs (avec webpack) va vous détailler la méthode que j’ai utilisé pour arriver à mes fins."
date:       2016-08-03 08:00:00
comments: true
header-img: "img/vuejs.png"
hearder-bg-color: "rgba(66, 185, 131, 0.9)"
---

# Vuejs - Une configuration différente pour chaque environnement

Lors du développement d'une application web basée sur VueJS, j'ai eu besoin d'appeler mon API via une url différente en fonction de mon environnement (local, preview et production).

Ce tuto sous vuejs (avec webpack) va vous détailler la méthode que j'ai utilisé pour arriver à mes fins.

## Prérequis

Cette méthode ne fonctionne qu'avec les applications VueJS basées sur webpack.

## Environnement

Voici ma configuration pour cette démo

- Une machine Windows 10
- NPM en version 5
- Vuejs en version 1.0.26 avec webpack

## Les URLS

Voici les différentes url de notre API en fonction de notre environnement :

- dev :  `http://myapi.dev`
- preview : `https://preview.myapi.com`
- prod : `https://myapi.com`

## Création des fichiers de configuration

Je vous invite à créer un dossier `config` dans `/src` qui sera composé de 3 sous dossiers `dev`, `preview` et `prod`

### Fichier de dev

Créer le fichier `src/config/dev/app.js`

{% highlight js %}
export const api = {
  baseUrl: 'http://myapi.dev'
}

{% endhighlight %}

### Fichier de preview

Créer le fichier `src/config/preview/app.js`

{% highlight js %}
export const api = {
  baseUrl: 'https://preview.myapi.com'
}

{% endhighlight %}

### Fichier de production

Créer le fichier `src/config/prod/app.js`

{% highlight js %}
export const api = {
  baseUrl: 'https://myapi.com'
}

{% endhighlight %}

> Et maintenant on fait comment pour charger le bon fichier ?

En fait on ne va jamais charger un fichier contenu dans ces 3 sous dossiers mais toujours charger le fichier `/src/config/app.js`

> Mais il n'existe pas ??!!

Oui c'est vrai mais on va voir comment le générer automatiquement.

## Générer automatiquement le bon fichier

On va se servir des commandes `npm run dev` et `npm run build` et créer la commande `npm run preview` pour effectuer cette action.

### ~~Dave~~ dev

Nous allons effectuer la copie du fichier `/src/config/dev/app.js` vers `/src/config/` avant de lancer notre serveur de dev lorsque nous exécuterons la commande `npm run dev`

Modifiez le fichier `/build/dev-server.js`

{% highlight js %}
// charger shelljs en début de fichier
require('shelljs/global')

// Après les différents "require" ajoutez la ligne suivante. Cette commande effectuera la copie de tous les fichiers du répertoire "dev" vers la racine du répertoire "config"
cp(path.resolve(__dirname, '../src/config/dev/*'), path.resolve(__dirname, '../src/config/'))
{% endhighlight %}

### Production

Nous allons effectuer la copie du fichier `/src/config/prod/app.js` vers `/src/config/` lorsque nous exécuterons la commande `npm run build`

{% highlight js %}
// Après les différents "require" ajoutez la ligne suivante. Cette commande effectuera la copie de tous les fichiers du répertoire "prod" vers la racine du répertoire "config"
cp(path.resolve(__dirname, '../src/config/prod/*'), path.resolve(__dirname, '../src/config/'))
{% endhighlight %}

### Preview

Là ce ne sera pas aussi simple que précédemment pour la bonne est simple raison que la commande `npm run preview` n'existe pas.
De plus on ne va pas simplement effectuer la copie de nos fichiers de config mais effectuer un `build` de notre application dans le répertoire `/preview` ne notre application plutôt que `/dist` afin d'éviter d'envoyer nos configurations de preview en production si on est pas trop attentif (mieux vaut être prudent).

#### Créer la commande

C'est dans le fichier `package.json` que ça ce passe. Vous devez ajouter le commande `preview` dans la partie `script`

{% highlight js %}
"scripts": {
  "dev": "node build/dev-server.js",
  "preview": "node build/preview.js",
  "build": "node build/build.js",
  ...
},
{% endhighlight %}

La commande `npm run preview` chargera le fichier `/build/preview.js`

#### Créer le fichier preview.js

Notre fichier sera une bête copie de `/build/build.js` sauf que la commande `cp` copiera les fichiers de `/src/config/preview/` à la place de `/src/config/prod/`.

{% highlight js %}
// https://github.com/shelljs/shelljs
require('shelljs/global')
env.NODE_ENV = 'production'

var path = require('path')
var config = require('../config')
var ora = require('ora')
var webpack = require('webpack')
var webpackConfig = require('./webpack.prod.conf')

cp(path.resolve(__dirname, '../src/config/preview/*'), path.resolve(__dirname, '../src/config/'))

console.log(
  '  Tip:\n' +
  '  Built files are meant to be served over an HTTP server.\n' +
  '  Opening index.html over file:// won\'t work.\n'
)

var spinner = ora('building for production...')
spinner.start()

var assetsPath = path.join(config.build.assetsRoot, config.build.assetsSubDirectory)
rm('-rf', assetsPath)
mkdir('-p', assetsPath)
cp('-R', 'static/', assetsPath)

webpack(webpackConfig, function (err, stats) {
  spinner.stop()
  if (err) throw err
  process.stdout.write(stats.toString({
    colors: true,
    modules: false,
    children: false,
    chunks: false,
    chunkModules: false
  }) + '\n')
})

{% endhighlight %}

> J'ai lancé la commande `npm run preview`, le fichier `config/app.js` est le bon mais le build n'a pas été fait dans le répertoire `preview`

Effectivement, il va falloir modifier notre config.

#### Build moi dans preview

La première chose à faire est de modifier le fichier `/config/index.js` afin d'ajouter la configuration `preview`. Par rapport à la config `build`, nous allons remplacer le répertoire `dist` par `preview`

{% highlight js %}
build: {
  ...
],
preview: {
  env: require('./prod.env'),
  index: path.resolve(__dirname, '../preview/index.html'),
  assetsRoot: path.resolve(__dirname, '../preview'),
  assetsSubDirectory: 'static',
  assetsPublicPath: '/',
  productionSourceMap: true
},
dev: {
  ...
}
{% endhighlight %}

La deuxième chose à faire est de créer un fichier `/build/webpack.preview.conf.js` qui va surcharger notre fichier `/build/webpack.prod.conf.js` afin d'utiliser la config `preview`

{% highlight js %}
var config = require('../config')
var merge = require('webpack-merge')
var baseWebpackConfig = require('./webpack.prod.conf')
var HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = merge(baseWebpackConfig, {
  output: {
    path: config.preview.assetsRoot,
    publicPath: config.preview.assetsPublicPath,
  },
  plugins: [
    new HtmlWebpackPlugin({
      filename: process.env.NODE_ENV === 'testing'
        ? 'index.html'
        : config.preview.index,
      template: 'index.html',
      inject: true,
      minify: {
        removeComments: true,
        collapseWhitespace: true,
        removeAttributeQuotes: true
        // more options:
        // https://github.com/kangax/html-minifier#options-quick-reference
      },
      // necessary to consistently work with multiple chunks via CommonsChunkPlugin
      chunksSortMode: 'dependency'
    })
  ]
})

{% endhighlight %}

Enfin il ne nous reste plus qu'à modifier notre fichier `/build/preview.js` pour utiliser la bonne configuration.

{% highlight js %}
// https://github.com/shelljs/shelljs
require('shelljs/global')
env.NODE_ENV = 'production'

var path = require('path')
var config = require('../config')
var ora = require('ora')
var webpack = require('webpack')
var webpackConfig = require('./webpack.preview.conf')

console.log(
  '  Tip:\n' +
  '  Built files are meant to be served over an HTTP server.\n' +
  '  Opening index.html over file:// won\'t work.\n'
)

var spinner = ora('building for preview...')
spinner.start()

cp(path.resolve(__dirname, '../src/config/preview/*'), path.resolve(__dirname, '../src/config/'))

var assetsPath = path.join(config.preview.assetsRoot, config.preview.assetsSubDirectory)
rm('-rf', assetsPath)
mkdir('-p', assetsPath)
cp('-R', 'static/', assetsPath)

webpack(webpackConfig, function (err, stats) {
  spinner.stop()
  if (err) throw err
  process.stdout.write(stats.toString({
    colors: true,
    modules: false,
    children: false,
    chunks: false,
    chunkModules: false
  }) + '\n')
})

{% endhighlight %}

## C'est la fin

Voilà le tuto touche à sa fin, si vous avez des remarques ou suggestions je vous invite à laisser un petit commentaire.