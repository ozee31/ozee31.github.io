---
layout: documentation
title: "OhMyCache - isExpired"
id: 'ohmycache'
section: "isExpired"
hearder-bg-color: "#323330"
---

<div class="row">
  <div class="col-md-2">
    {% include ohmycache/nav.html %}
  </div>

  <section class="col-md-10">
    <h1>isExpired</h1>
    <p>Indicates whether the item expired</p>
    {% highlight js %}isExpired(key){% endhighlight %}

    <h2>Parameters</h2>
    <dl class="dl-horizontal">
      <dt>key</dt>
      <dd>{string} name of the key you want to check</dd>
    </dl>

    <h2>Return</h2>
    <p>This method return a boolean</p>

    <h2>Exemples</h2>
{% highlight js %}
// No expiration
OhMyCache.Local.set('key', 'value')
OhMyCache.Local.isExpired('key') // false

// No expired
OhMyCache.Local.set('key', 'value', {expire: 999999})
OhMyCache.Local.isExpired('key') // false

// Expired
OhMyCache.Local.set('key', 'value', {expire: 1})
// sleep 1 second or more
OhMyCache.Local.isExpired('key') // true
{% endhighlight %}
  </section>
</div>