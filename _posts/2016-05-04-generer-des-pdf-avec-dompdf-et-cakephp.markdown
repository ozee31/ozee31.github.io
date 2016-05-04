---
layout:     post
title:      "Générer des PDF avec DomPDF et CakePHP"
subtitle:   "Générer des PDF sous CakePHP n'a jamais été aussi symple."
date:       2016-05-04 12:00:00
header-img: "img/post-bg-1.jpg"
---

# Générer des PDF avec DomPDF et CakePHP
Grâce à la librairie [DomPDF](https://github.com/dompdf/dompdf) couplé au plugin [cakephp-dompdf](https://github.com/DaoAndCo/cakephp-dompdf), il est très facile de générer des PDF en HTML/CSS.

## Environnement
Voici ma configuration pour cette démo

- Une VM ubuntu server en version 14
- Server Apache
- PHP en version 7 (la version minimale est la 5.4.16)
- CakePHP en version 3.2 (la version minimale est la 3.0)
- La librairie DomPDF en version 0.7 beta (la 0.6 n'utilise pas les namespaces)
- Le plugin cakephp-dompdf en version 1.0

## Installation
Pour installer la librairie ainsi que le plugin, rien de plus simple en utilisant composer

```SHELL
    composer require dompdf/dompdf:0.7.x@beta
    composer require daoandco/cakephp-dompdf
```

Très important, il ne faut pas oublié de générer les liens symboliques du plugin.

```SHELL
    bin/cake plugin assets symlink
```

On charge ensuite le plugin

```PHP
    // config/bootstrap.php

    Plugin::load('Dompdf');
```

## En route pour l'aventure
Le plus simple pour générer les PDF est d'écouter les urls afin de traiter l'extension `.pdf` ([Documentation](http://book.cakephp.org/3.0/fr/development/routing.html#routing-des-extensions-de-fichier))

On va donc, pour l'exemple définir une ~~auto~~ route.

```PHP
Router::scope('/', function (RouteBuilder $routes) {
    $routes->extensions(['pdf']);
    $routes->connect('/demo/view/*', ['controller' => 'Demo', 'action' => 'view']);
}
```

Et pour le controlleur :

```PHP
// scr/Controller/DemoController.php

namespace App\Controller;

class DemoController extends AppController {

    public function view($name) {

    }
}
```

On teste notre route http://localhost/demo/view/test.pdf et **bim!!!**

![Template is missing](https://lh3.googleusercontent.com/87TellwMnUAMrKZfakHSiIqnmuxd9fGbRzKS4B--_XAqaC7X9wDa00-i5rFXesNMtaWSrGcY8Q=s0 "capture-1.JPG")

Ne vous inquiétez pas, c'est normal, pas de panique on a pas encore créé notre template.

## J'ai view un PDF

Comme on écoute toujours la maîtresse et que l'on est bien sage, on va créer notre fichier `view.ctp` dans `Demo/pdf/view.ctp`

```HTML
<!-- src/Template/Demo/pdf/view.ctp -->

<h1>Ceci est un pédéhèffe</h1>

<p>Il est généré avec Dôme Pédéhèffe</p>
```

> Hey, t'est bien gentil mais là il me demande de créer un layout.

Oui je sais mais comme la vie est bien faite, notre petit plugin inclut justement un Layout qui nous permet entre autre de fixer un header ainsi qu'un footer.

## Plugue ton Layout

Pour faire les choses bien (il faut toujours faire les choses bien), on va modifier notre Controller

```PHP
public function view($name) {

    $this->viewBuilder()
        ->className('Dompdf.Pdf')
        ->layout('Dompdf.default')
        ->options(['config' => [
            'filename' => $name,
            'render' => 'browser',
    ]]);
}
```

On recharge la page, et là, sous vos yeux ébahis, un magnifique PDF apparaît dans votre Firefox (comment ça vous n'utilisez pas Firefox ?)

![Le pdf généré](https://lh3.googleusercontent.com/tJcFxm3bITPAfJTdhNHRi6uTtsMGIdxvIFhe9xNq3yJWwIF2TkqI8IcFItt7Upx2MHPvRC7sqA=s0 "capture-2.JPG")

> Tout ça juste pour charger un Layout ?

En fait on a surtout dit à CakePHP d'utiliser le plugin pour générer la vue, je vous invite à lire [la documentation du plugin](https://github.com/DaoAndCo/cakephp-dompdf#configuration) pour comprendre ce que je viens de faire.

## Je veux du CSS et des nanas

> Bon les h1 et les p c'est bien gentil mais comment je style ça ? Et les images comment ça marche ?

On y viens, et on va même faire un header et un footer comme je suis gentil.

Allez c'est parti on crée notre beau css

```CSS
/* webroot/css/demo.css */

h1 {
    color: red;
}
```

Et comme tout est prévu, notre beau petit plugin possède un helper, on peut donc modifier notre fichier `view.ctp`

```HTML
<!-- src/Template/Demo/pdf/view.ctp -->

<?= $this->Dompdf->css('demo'); ?>

<h1>Ceci est un pédéhèffe</h1>

<p>Il est généré avec Dôme Pédéhèffe</p>
```

Ah oui, j'oubliais les nanas, pour les images c'est aussi avec le helper (et on en profite pour rajouter le header et le footer)

```HTML
<!-- src/Template/Demo/pdf/view.ctp -->

<?= $this->Dompdf->css('demo'); ?>

<?php
$this->start('header');
    echo '<p>I\'m a header</p>';
$this->end();

$this->start('footer');
    echo '<p>I\'m a footer</p>';
$this->end();
?>

<h1>Ceci est un pédéhèffe</h1>

<p>Il est généré avec Dôme Pédéhèffe</p>

<?= $this->Dompdf->image('nanas.jpg'); ?>
```

On actualise la page, et voilà notre PDF stylé comme jamais
![enter image description here](https://lh3.googleusercontent.com/4zy5TzBYp6cT4PDG_S7-V7yzX9J5BIyze5Uj-fHTpJBN7XY0NymbLGRjkvQedxVNIx9Dm6RYwg=s0 "capture-3.JPG")

## Aller plus loin
Je vous invite à lire la [documentation](https://github.com/DaoAndCo/cakephp-dompdf) du plugin qui est très complète.