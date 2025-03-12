---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Form HTML File

Biruni HTML forms follow a specific structure, including a designated script tag and layout elements:

```html
<script biruni></script>

<div class="b-toolbar"></div>

<div class="b-content"></div>
```

## Script

Including the `biruni` attribute in the `<script>` tag grants access to the `bBasePage` factory, which is essential for handling page logic.

JavaScript logic of the HTML code is written in this script tag. But we need to connect JavaScript with our HTML code first. Here is how page controller is created:

```
<script>
page.ctrl(function() {

});
</script>
```

`page.ctrl` recieves function as an argument. We can inject dependencies inside the function and use them in our  controller.

Common dependencies in `page.ctrl`&#x20;

* **scope** — Establishes the connection between JavaScript and HTML. It applies specifically to the `b-toolbar` and `b-content` of the current form.
* **model** — On page initialization, an automatic request is sent to the backend (`:model` action path), and the response is stored in this object.
* **fi** — Manages role-based access control for [form actions](./#form-actions)
* **t** — Enables [translate](../translate.md) functionality within JavaScript

## HTML

Both `b-toolbar` and `b-content` are inside the controller scope and are used as follows based on their purpose:

* **b-toolbar** — Typically contains buttons and grid controllers, positioned at the top of `b-content`.
* **b-content** — Holds the main page content, allowing dynamic page creation using Biruni's built-in directives.

### Putting all together

Below is a sample code demonstrating the connection between JavaScript and HTML using scope:

```html
<script biruni>
page.ctrl(function(scope) {
  var q = { names: ["tom", "mark"] };

  function show(name) {
    q.name = name;
  }

  scope.q = q;
});
</script>
<div class="b-toolbar">
  <button type="button" ng-click="show(q.names[0])">Button {{ q.names[0] }}</button>
  <button type="button" ng-click="show(q.names[1])">Button {{ q.names[1] }}</button>
</div>
<div class="b-content">
  <h1 ng-if="q.name">{{ q.name }}</h1>
</div>
```
