---
layout: documentation
title: "OhMyCache - keys"
id: 'ohmycache'
section: "keys"
hearder-bg-color: "#323330"
---

<div class="row">
  <div class="col-md-2">
    {% include ohmycache/nav.html %}
  </div>

  <section class="col-md-10">
    <h1>keys</h1>
    <p>Return all item's keys, doen't remove expired items</p>
    {% highlight js %}keys(){% endhighlight %}

    <h2>Return</h2>
    <p>All item's keys (array)</p>

    <h2>Exemples</h2>
{% highlight js %}
OhMyCache.Local.set('k1', 'v1')
OhMyCache.Local.set('k2', 'v2', {readonly: true})
OhMyCache.Local.set('k3', 'v3', {expire: 1})

// sleep 2 secondes or more
OhMyCache.Local.keys() // ['k1', 'k2', 'k3']
{% endhighlight %}
  </section>
</div>