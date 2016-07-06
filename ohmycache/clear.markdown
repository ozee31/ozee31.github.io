---
layout: documentation
title: "OhMyCache - clear"
id: 'ohmycache'
section: "clear"
hearder-bg-color: "#323330"
---

<div class="row">
  <div class="col-md-2">
    {% include ohmycache/nav.html %}
  </div>

  <section class="col-md-10">
    <h1>clear</h1>
    <p>Remove all items. Also removes read-only datas</p>
    {% highlight js %}clear(){% endhighlight %}

    <h2>Return</h2>
    <p>This method return a boolean</p>

    <h2>Exemples</h2>
{% highlight js %}
OhMyCache.Local.set('key', 'value')
OhMyCache.Local.set('key2', 'value', {readonly: true})

OhMyCache.Local.clear() // true

OhMyCache.Local.get('key') // null
OhMyCache.Local.get('key2') // null
{% endhighlight %}
  </section>
</div>