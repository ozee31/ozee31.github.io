---
layout: documentation
title: "OhMyCache - Changelog"
id: 'ohmycache'
section: "changelog"
hearder-bg-color: "#323330"
---

<div class="row">
  <div class="col-md-2">
    {% include ohmycache/nav.html %}
  </div>

  <section class="col-md-10">
    <h1>Changelog</h1>
    <p>All Notable changes will be documented in this file</p>

    <h2>1.1.0</h2>
    <h3>Add functions</h3>
    <dl class="dl-horizontal">
      <dt>keys()</dt>
      <dd>Return all item's keys</dd>
      <dt>getAll()</dt>
      <dd>Get all values, remove if expired</dd>
    </dl>

    <h3>Update functions</h3>
    <dl class="dl-horizontal">
      <dt>clear()</dt>
      <dd>Add onlyExpired parameter for remove only expired items</dd>
    </dl>

    <h2>1.0.0</h2>
    <p>Initial Release</p>

  </section>
</div>