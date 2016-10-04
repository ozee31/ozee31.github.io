---
layout:     post
title:      "[Electron] - Créer une application native en HTML/JS"
subtitle:   "Créer une application native multi OS est devenu très simple grâce à Electron. En effet plus besoin d'apprendre le C, C++, Java... il est désormais possible de développer votre application avec les technologies du Web."
date:       2016-10-04 08:00:00
comments: true
header-img: "img/electron.png"
hearder-bg-color: "rgba(43, 46, 59, 0.9)"
---

# Electron - Créer une application native en HTML/JS

Créer une application native multi OS est devenu très simple grâce à Electron. En effet plus besoin d'apprendre le C, C++, Java... il est désormais possible de développer votre application avec les technologies du Web.

## Présentation

Initialement développé pour l'éditeur Atom, l'outil de Github a été utilisé pour créer des applications par des entreprises comme Microsoft, Facebook, Slack et Docker.

Pour fonctionner, Electron utilise NodeJS et Chromium et fourni une API afin de gérer les fenêtres, communiquer avec le système...

## Préparation

La première chose à faire est de démarrer un projet NPM.

{% highlight shell %}
npm init
{% endhighlight %}

Installez maintenant Electron en tant que dépendance de développement

{% highlight shell %}
npm i -D electron
{% endhighlight %}

Nous allons maintenant structurer notre projet, créez les répertoires suivant :

- **dist** : contiendra les fichiers natif (Windows, Linus, MacOS)
- **src** : contiendra le code source de notre application

Créez ensuite les fichiers suivant :

- **/main.js** : c'est le fichier central, celui qui va générer l'application electron
- **/src/index.html** : la page d'accueil de notre application
- **/src/app.js** : le code javascript de notre application sera stocké ici
- **/src/app.css** : le style css de notre application sera stocké ici

Attention : dans votre fichier `package.json` la valeur de la clé `main` doit pointer sur `main.js`

```
"main": "main.js",
```

## Allons y les z'amis

Nous allons commencer par un exemple des plus basique

### main.js

Le code du fichier `main.js` est issu du tuto [démarrage rapide](https://github.com/electron/electron-quick-start)

{% highlight js %}
const electron = require('electron')

// Permet de gérer les evenements système
const app = electron.app

// Permet de gérer les fenetres
const BrowserWindow = electron.BrowserWindow

// Gardez une référence globale de l'objet "window", si vous ne le faites pas, la fenêtre se ferme automatiquement lorsque l'objet JavaScript est nettoyé.
let mainWindow

function createWindow () {
  // Création d'une fenetre en résolution 800x600
  mainWindow = new BrowserWindow({width: 800, height: 600})

  // La fenetre va charger notre fichier index.html
  mainWindow.loadURL(`file://${__dirname}/src/index.html`)

  // Cet evenement est déclenché lorsque la fenetre est fermée
  mainWindow.on('closed', function () {
    // réinitialisation de l'objet "window"
    mainWindow = null
  })
}

// Cet évenement est déclenché lorsque Electron a terminé son initialisation. On lance la création de la fenêtre à ce moment là
app.on('ready', createWindow)

// Cet évenement est déclenché lorque toutes les fenêtres sont fermées (pour notre exemple nous n'avons qu'une seule fenêtre mais il est possble de gérer le multi-fentres)
app.on('window-all-closed', function () {
  // Sur OS X, il est courant que les applications puissent rester active jusqu'à ce que l'utilisateur quitte explicitement via la commande "Cmd + Q"
  if (process.platform !== 'darwin') {
    app.quit()
  }
})

app.on('activate', function () {
  // Sur OS X, il est courant de re-créer une fenêtre dans l'application lorsque l'icône du dock est cliqué et il n'y a pas d'autres fenêtres ouvertes.
  if (mainWindow === null) {
    createWindow()
  }
})
{% endhighlight %}

### index.html

Notre ~~super~~ application nous permettra de générer un nombre aléatoire entre 0 et 100 (oui monsieur). On aura même un bouton pour quitter l'application (oui car le `alt + F4` c'est marrant 2 secondes mais pas plus).

{% highlight html %}
<!doctype html>
<html lang="fr">
<head>
  <meta charset="utf-8">
  <title>Electron Demo</title>
  <link rel="stylesheet" href="app.css">
</head>

<body>
  <h1>Electron Demo</h1>

  <section>
    <h1>Géréner un nombre aléatoire</h1>

    <div>
      Valeur =
      <span id="random-result"></span>
    </div>
    <button id="random-button" type="button">Générer</button>
  </section>

  <button id="app-close" type="button">Fermer l'application</button>

  <script src="app.js"></script>
</body>
</html>
{% endhighlight %}

### app.css

On ajoute un style tout droit sorti du futur

{% highlight css %}
h1 {
  color: red;
}

#app-close {
  position: absolute;
  right: 5px;
  top: 5px;
}
{% endhighlight %}

### app.js

Et maintenant on code notre application ~~révolutionnaire~~.

{% highlight js %}
window.addEventListener('load', function load(event) {

  var randomResult = document.getElementById('random-result')

  document.getElementById('random-button').onclick = function(event) {
    randomResult.textContent = Math.floor(Math.random() * 100)
  }

  document.getElementById('app-close').onclick = function(event) {
    // Ici on utilisera l'api pour fermer l'application
  }
})
{% endhighlight %}

### On teste

Pour tester rien de plus simple, il suffit de lancer la commande `electron .`

Si tout va bien, l'application devrait se lancer en mode fenêtre. Bien sur le bouton "Fermer l'application" ne fonctionne pas (car la fonctionnalité n'a pas été codé).

## On passe à la vitesse supérieure (pas trop quand même)

### Fermer l'application

Pour fermer l'application nous allons utiliser l'API d'electron afin de faire communiquer l'objet `window` avec notre fichier `main.js`.

Je vous invite donc à modifier le fichier `app.js` comme ceci.

{% highlight js %}
document.getElementById('app-close').onclick = function(event) {
  const remote = require('electron').remote // chargement de l'api remote
  var window = remote.getCurrentWindow() // on récupère la fenetre courante
  window.close() // on ferme la fenetre
 }
{% endhighlight %}

Maintenant si vous relancez electron et que vous cliquez sur "Fermer l'application" ... L'application se ferme #magie

### Plein écran

Si vous voulez passer votre application en plein écran, vous pouvez modifier le fichier `main.js` comme ceci :

{% highlight js %}
mainWindow = new BrowserWindow({fullscreen: true})
{% endhighlight %}

### Compiler

Bon voilà notre application est terminée, elle est belle et sans bug, comment on compile?

Un outil fait tout le travail (ou presque), il s'agit d'[electron-packager](https://github.com/electron-userland/electron-packager)

Il faut dans un premier temps l'installer

{% highlight shell %}
npm i -D electron-packager
{% endhighlight %}

Et enfin on lance la compilation

{% highlight shell %}
electron-packager . demo --out dist --overwrite --all
{% endhighlight %}

La commande va compiler le dossier courant `.` dans le répertoire `dist`, va le nommer `demo`, le compilera pour toutes les plateformes `--all`, et enfin forcera la recompilation si le projet existe déjà `--overwrite`.

Vous pouvez maintenant trouver dans le répertoire "dist" l'ensemble des déclinaisons de votre projet pour les différents systèmes d'exploitations.

## Aller plus loin

- Lire la [documentation](http://electron.atom.io/docs/)
- Démarrer vos projets grâce aux [Boilerplates de la communauté](http://electron.atom.io/community/#boilerplates) (react, vuejs...)