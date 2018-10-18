# Routing
Now that we know how to create our own components, we need to start working on the **navigation between them**. We'll start by learning how **routing** can help us handle it.

Routing is widely used in modern web development. When we interact with a web page, we're used to see the URL in the browser change and load different content. That's **routing**: loading a different part of our application **depending on the URL and data sent in the request**.

###### Basic routing: page not found
For handling routing in Polymer we're going to use **app-router**. Let's install it using bower, like so:

    bower install app-router --save

We are going to add **basic routing** to our app that:

* Shows the home page.
* If any other path is selected, shows a page not found page.

Let's start by **creating the page-not-found component**. For that we'll use our usual approach of **duplicating an already existing component**, linking it in the index page and checking if duplication was ok.

Code for the component.

```html
<!-- page-not-found.html -->
<link rel="import" href="../../bower_components/polymer/polymer-element.html">

<dom-module id="page-not-found">
  <template>
    <style>
      :host {
        display: block;
      }
    </style>
    <h2>Aquí no hay nada que ver</h2>

  </template>

  <script>
    /**
     * @customElement
     * @polymer
     */
    class PageNotFound extends Polymer.Element {
      static get is() { return 'page-not-found'; }
      static get properties() {
        return {
        };
      }
    }

    window.customElements.define(PageNotFound.is, PageNotFound);
  </script>
</dom-module>
```

Now import & link in index.html and **check it's shown**; **then remove the component from the html** but not the import!.

Now for a little more theory on routing. We'll first **register the routes we want to make available**. When the user enters a URL in the browser, the router component will try to find a match among all the registered routes **starting from the first and going sequentially to the last**. When there's a match, the **component is loaded**.

This sequential processing allows us to add a **"default" route in case there was no match**. And, you've guessed it, this is where our **page-not-found component comes into action**.

On to the routing then!.

```html
<!-- Import app-router -->
<link rel="import" href="/bower_components/app-router/app-router.html">
<!-- Index.html -->
<h1> Enrutamiento <h1>
<app-router>
  <app-route path="/"></app-route>
  <app-route path="*" import="/src/page-not-found/page-not-found.html"></app-route>
</app-router>
```

We have two routes:
* A "home"
* A default page for anything that is not the home or other route registered, hence the asterisk.

**Try this in the browser!**. If you enter just the main component path, **you'll see the welcome message** if you enter, say, /whatever **you'll see the page not found message**.

###### Routing the visor persona

So, now that we have a basic routing in our pocket, let's make things a bit more interesting.

We're going to link one of the components we've made **visor-persona**.

```html
<!-- Import app-router -->
<link rel="import" href="/bower_components/app-router/app-router.html">
<!-- Index.html -->
<h1> Enrutamiento <h1>
<app-router>
  <app-route path="/"></app-route>
  <app-route path="/person" import="/src/visor-persona/visor-persona.html"></app-route>
  <app-route path="*" import="/src/page-not-found/page-not-found.html"></app-route>
</app-router>
```

That was easy right?. Check it works on the browser. With this **we've covered basic routing!**.

And this is it for this lab, see you at the next, and last of the course!.
