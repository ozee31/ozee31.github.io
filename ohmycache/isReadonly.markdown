---
layout: documentation
title: "OhMyCache - isReadonly"
id: 'ohmycache'
section: "isReadonly"
hearder-bg-color: "#323330"
---

<div class="row">
  <div class="col-md-2">
    {% include ohmycache/nav.html %}
  </div>

  <section class="col-md-10">
    <h1>isReadonly</h1>
    <p>Indicates whether the value is readonly</p>
    {% highlight js %}isReadonly(key){% endhighlight %}

    <h2>Parameters</h2>
    <dl class="dl-horizontal">
      <dt>key</dt>
      <dd>{string} name of the key you want to check</dd>
    </dl>

    <h2>Return</h2>
    <p>This method return a boolean</p>

    <h2>Exemples</h2>
{% highlight js %}
// No readonly
OhMyCache.Local.set('key', 'value')
OhMyCache.Local.isReadonly('key') // false

// is readonly
OhMyCache.Local.set('key', 'value', {readonly: true})
OhMyCache.Local.isReadonly('key') // true
{% endhighlight %}
  </section>
</div>