---
layout:     post
title:      "[php] CakePHP 3 : Activer le CrossDomain (CORS)"
subtitle:   "Si vous créez une API publique sous CakePHP 3 vous aurez besoin d'accepter les requêtes CrossDomain. Voilà comment faire."
date:       2016-07-12 08:00:00
comments: true
header-img: "img/php.png"
hearder-bg-color: "#6082BB"
---

> **!!!**
>
> **Attention cette méthode est dépréciée depuis la version 3.3 de CakePHP, je vous invite [à lire cet article](http://flavienbeninca.fr/2016/11/09/cakephp-3-3-crosscomain-cors-middleware.html) pour activer le CrossDomain grâce au Middleware.**
>
> **!!!**

# CakePhp 3 : Activer le CrossDomain (CORS)

Suite à la création d'une API sous CakePHP 3.2, j'ai du à un moment autoriser le CrossDomain afin que mon API soit consultable depuis n'importe quel domaine.

Quelle ne fut pas ma surprise de découvrir que le framework ne permettait pas de le faire de manière simple et efficace.

## Filtre du Dispatcher

La première chose à faire est d'ajouter un filtre de dispatcher afin d'autoriser le CrossDomain sur votre application CakePhp.

Si vous ne savez pas ce qu'est un filtre de dispatcher vous pouvez toujours consulter la [documentation](http://book.cakephp.org/3.0/fr/development/dispatch-filters.html).

### Création de la classe RestFilter

La classe doit être créée dans `src/Routing/Filter/RESTFilter.php`

{% highlight php %}
<?php
namespace App\Routing\Filter;

use Cake\Event\Event;
use Cake\Routing\DispatcherFilter;

class RESTFilter extends DispatcherFilter {

  public function beforeDispatch(Event $event) {
    $request = $event->data['request'];
    $response = $event->data['response'];

    $origin = $request->header('Origin');

    if (!empty($origin)) {
      $response->header('Access-Control-Allow-Origin', $origin);
    }

    if ($request->method() == 'OPTIONS') {
      $method  = $request->header('Access-Control-Request-Method');
      $headers = $request->header('Access-Control-Request-Headers');
      $response->header('Access-Control-Allow-Headers', $headers);
      $response->header('Access-Control-Allow-Methods', empty($method) ? 'GET, POST, PUT, DELETE' : $method);
      $response->header('Access-Control-Allow-Credentials', 'true');
      $response->header('Access-Control-Max-Age', '120');
      $response->send();
      die;
    }
  }
}
{% endhighlight %}

### Activer le filtre

Maintenant on active le filtre dans `bootstrap.php` en ajoutant cette ligne

{% highlight php %}
<?php
DispatcherFactory::add('REST', ['priority' => 1]);
{% endhighlight %}

## Le cas des exceptions

Alors que je pensais que le travail était fini, j'ai découvert que lorsque je lançais des exeptions, il m'était impossible côté front d'analyser la réponse.

En effet sous firefox en analysant la réponse j'ai un petit message :
![SyntaxError](https://lh3.googleusercontent.com/O9fYwu03Z4XLp0MogPOHEHxU6OcgAg8bdnce9duwRq9xNt1uGo9HniH1KfwyDF0YMPf0XJFh=s0 "exception-response.JPG")

Du coup impossible pour moi en JS de savoir quel type d'erreur je reçoit (401, 404, ... ?), quel est le message d'erreur, l'url...

Après quelques recherches et manipulations j'ai découvert que les exceptions redéfinissait les headers de la réponse et donc annulaient ceux définit dans le filtre du Dispatcher.

La solution que j'ai trouvée est de surcharger la classe `ErrorHandler` afin de redéfinir les headers dans la fonction `_sendResponse()`

### Création de la classe de surcharge

La classe doit être écrite dans `src/Error/AppError.php`

{% highlight php %}
<?php
namespace App\Error;

use Cake\Error\ErrorHandler;

class AppError extends ErrorHandler {

  protected function _sendResponse($response) {
    $response->header('Access-Control-Allow-Origin','*');
    $response->header('Access-Control-Allow-Methods','*');
    $response->header('Access-Control-Allow-Headers','X-Requested-With');
    $response->header('Access-Control-Allow-Headers','Content-Type, x-xsrf-token');
    $response->header('Access-Control-Max-Age','120');

    parent::_sendResponse($response);
  }
}
{% endhighlight %}


### Activer la classe

Il va maintenant falloir activer cette classe à la place de celle par défaut et ça ce passe dans `bootstrap.php`

{% highlight php %}
<?php
// remplacer "use Cake\Error\ErrorHandler;" par

use App\Error\AppError;

// remplacer "(new ErrorHandler(Configure::read('Error')))->register();" par

(new AppError(Configure::read('Error')))->register();
{% endhighlight %}

Et voilà maintenant les exceptions pourront être facilement traitées côté front

## Allez plus loin

Je pense qu'il peut être judicieux de créer un plugin pour ça afin de pouvoir réutiliser le code sur plusieurs projets et ceux sans prise de tête.