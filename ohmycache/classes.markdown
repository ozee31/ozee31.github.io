---
layout: documentation
title: "OhMyCache - Classes"
id: 'ohmycache'
section: "classes"
hearder-bg-color: "#323330"
---

<div class="row">
  <div class="col-md-2">
    {% include ohmycache/nav.html %}
  </div>

  <section class="col-md-10">
    <h1>Classes</h1>

    <p>
      OhMyCache.Local and OhMyCache.Session share the same API, have access to the same functions.
    </p>

    <h2>OhMyCache.Local (use localStorage)</h2>
    <p>
      The localStorage property allows you to access a local Storage object. localStorage is similar to sessionStorage. The only difference is that, while data stored in localStorage has no expiration time, data stored in sessionStorage gets cleared when the browsing session ends—that is, when the browser is closed.
    </p>
    <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage" title="MDN">More infos</a>

    <h2>OhMyCache.Session (use sessionStorage)</h2>
    <p>
      The sessionStorage property allows you to access a session Storage object. sessionStorage is similar to Window.localStorage, the only difference is while data stored in localStorage has no expiration set, data stored in sessionStorage gets cleared when the page session ends. A page session lasts for as long as the browser is open and survives over page reloads and restores. Opening a page in a new tab or window will cause a new session to be initiated, which differs from how session cookies work.
    </p>
    <a href="https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage" title="MDN">More infos</a>
  </section>
</div>