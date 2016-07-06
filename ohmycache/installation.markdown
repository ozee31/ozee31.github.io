---
layout: documentation
title: "OhMyCache - Installation"
id: 'ohmycache'
section: "installation"
hearder-bg-color: "#323330"
---

<div class="row">
  <div class="col-md-2">
    {% include ohmycache/nav.html %}
  </div>

  <section class="col-md-10">
    <h1>Installation</h1>

    <h2>Download</h2>

    <h3>NPM</h3>
    {% highlight shell %}npm install OhMyCache{% endhighlight %}

    <h3>Bower</h3>
    {% highlight shell %}bower install OhMyCache{% endhighlight %}


    <h2>Load the library</h2>

    <h3>ES2015 (babel)</h3>
{% highlight js %}
import {Local, Session} from 'OhMyCache'

// quick use
Local.set('key', 'val')
Session.set('key', 'val')
{% endhighlight %}

    <h3>require function of node</h3>
{% highlight js %}
var OhMyCache = require('OhMyCache')

// quick use
OhMyCache.Local.set('key', 'val')
OhMyCache.Session.set('key', 'val')
{% endhighlight %}

    <h3>Html script</h3>
{% highlight html %}
<html>
  <head>
    <!-- npm -->
    <script src="node_modules/ohmycache/dist/bundle.js"></script>

    <!-- bower -->
    <script src="bower_components//ohmycache/dist/bundle.js"></script>

    <!-- other method -->
    <script src="lib-path/ohmycache/dist/bundle.js"></script>
    <script>
      // quick use
      OhMyCache.Local.set('key', 'val')
      OhMyCache.Session.set('key', 'val')
    </script>
  </head>
</html>
{% endhighlight %}
  </section>
</div>
