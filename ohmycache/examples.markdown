---
layout: documentation
title: "OhMyCache - Exemples"
id: 'ohmycache'
section: "exemples"
hearder-bg-color: "#323330"
---

<div class="row">
  <div class="col-md-2">
    {% include ohmycache/nav.html %}
  </div>

  <section class="col-md-10">
      <h1>Exemples</h1>

      <h2>Store data in local storage</h2>
      {% highlight js %}OhMyCache.Local.set('key', 'local'){% endhighlight %}

      <h2>Store data in session storage</h2>
      {% highlight js %}OhMyCache.Session.set('key', 'session'){% endhighlight %}

      <h2>Get data in local storage</h2>
      {% highlight js %}OhMyCache.Local.get('key') // return 'local'{% endhighlight %}

      <h2>Get data in session storage</h2>
      {% highlight js %}OhMyCache.Session.get('key') // return 'session'{% endhighlight %}

      <h2>Store an object</h2>
      {% highlight js %}OhMyCache.Local.set('myObj', {attr1: 'val1', attr2: 'val2'}){% endhighlight %}
  </section>
</div>
