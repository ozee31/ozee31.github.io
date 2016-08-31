---
layout: documentation
title: "OhMyCache - getAll"
id: 'ohmycache'
section: "getAll"
hearder-bg-color: "#323330"
---

<div class="row">
  <div class="col-md-2">
    {% include ohmycache/nav.html %}
  </div>

  <section class="col-md-10">
    <h1>getAll</h1>
    <p>Get all items, remove all expired items</p>
    {% highlight js %}getAll(){% endhighlight %}

    <h2>Return</h2>
    <p>All items like {key: value, key2: value2...} (object)</p>

    <h2>Exemples</h2>
{% highlight js %}
OhMyCache.Local.set('k1', 'v1')
OhMyCache.Local.set('k2', 'v2', {readonly: true})
OhMyCache.Local.set('k3', 'v3', {expire: 1})

// sleep 2 secondes or more
OhMyCache.Local.getAll() // {k1: 'v1', k2: 'v2'}
{% endhighlight %}
  </section>
</div>