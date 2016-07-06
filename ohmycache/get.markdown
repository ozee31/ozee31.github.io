---
layout: documentation
title: "OhMyCache - get"
id: 'ohmycache'
section: "get"
hearder-bg-color: "#323330"
---

<div class="row">
  <div class="col-md-2">
    {% include ohmycache/nav.html %}
  </div>

  <section class="col-md-10">
    <h1>get</h1>
    <p>Get the value, remove the date if expired</p>
    {% highlight js %}get(key){% endhighlight %}

    <h2>Parameters</h2>
    <dl class="dl-horizontal">
      <dt>key</dt>
      <dd>{string} name of the key you want to read</dd>
    </dl>

    <h2>Return</h2>
    <p>This method return the value of a data (string, int, float, array, object...)</p>

    <h2>Exemples</h2>
{% highlight js %}
OhMyCache.Local.set('key', 'value')

OhMyCache.Local.get('key') // 'value'
OhMyCache.Session.get('key') // null

OhMyCache.Local.set('key', 'value', {expire: 1})
// sleep 2 secondes or more
OhMyCache.Local.get('key') // null
{% endhighlight %}
  </section>
</div>