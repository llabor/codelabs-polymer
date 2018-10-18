## Iron Pages

Another way of dealing with component navigation is **content switching**. Usually we're going to have **many different web components** in a single page. Choosing when to show or hide one component -or a group- can be tricky, **specially as the number of components grows**.

That's where **iron pages** comes to the rescue: it allows us to **easily switch between components** in the same page.

It's important to note that routing and content switching  **are not mutually exclusive**. It's perfectly OK to have a route load a parent web component that manages many children components using iron pages.
One of those components could link to a different route,
that loads another component that uses iron-pages, and so on.

###### Iron pages test

So, first of all let's **install iron-pages** using bower

    bower install iron-pages --save

And then create a new **component named test-iron-pages**, as always, duplicating another.

Also let's make sure **we import iron-pages**.

```html
<link rel="import" href="../../bower_components/iron-pages/iron-pages.html">
```

And add a property, **componentName**, more on this in a minute.

```javascript
// Don't forget the <script> tags!.
class TestIronPages extends Polymer.Element {
  static get is() { return 'test-iron-pages'; }
  static get properties() {
    return {
      componentName: {
        type: String
      }
    };
  }
}
window.customElements.define(TestIronPages.is, TestIronPages);
```

Check everything is working by **importing and adding test-iron-pages in index.html**.

So, now that we have laid down the foundations, let's see how iron-pages works. As we've said, iron-pages helps us with **content switching**. It works like this:

* Uses a **property value** to choose which content to show. This value is set in the **selected attribute of the iron-pages component**.
* The selected attribute value must match **the value of an attribute in the component to show**.
* The name of the attribute which value has to match selected's value, is set **in the attr-for-selected attribute of the iron-pages component**.

It much easier than it seems, we'll see it much clearer with an example.

Let's **add this HTML to the template**.

```html
<!-- test-iron-pages.html -->
<!-- Import components -->
<link rel="import" href="../visor-persona/visor-persona.html">
<link rel="import" href="../page-not-found/page-not-found.html">

<!-- Inside the template -->
<h1>IRON PAGES TEST</h1>
<input value="{{componentName::input}}" placeholder="Select component">
<iron-pages selected="[[componentName]]" attr-for-selected="component-name">
  <div component-name="visor-persona"><visor-persona></visor-persona></div>
  <div component-name="page-not-found"><page-not-found ></page-not-found></div>
</iron-pages>
```

OK, **so how does this work?**.

* We enter a value in the Select Component input.
* That input is bound, two-way, to the componentName property. Let's say we enter "visor-persona". Now **componentName** value is "visor-persona".
* As you can see in this code here **<iron-pages selected="[[componentName]]"** the value of the selected attribute in iron-pages now is "visor-persona" too, because it's bound one way.
* In iron-pages we have **attr-for-selected="component-name"**. That means that if anything that is **between the <iron-pages> tags** has an attribute named **component-name** that has the same value as **selected** has now, is going to be shown, anything else will be hidden.
* At this moment, we do have a match!, because **selected has a value of "visor-persona"** and we do have a div with an attribute like **component-name="visor-persona"** so it'll be shown.
* And, this does the trick, what's inside that div is **the visor-persona component itself**, that will appear in all it's glory!.

Try and **change the value of the input** and **see how components appear/disappear**.
