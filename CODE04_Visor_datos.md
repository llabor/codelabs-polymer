Hello and welcome! In this codelab **we're going to create a Polymer component** and then use it to **print some data about the course**.

Let's get going!.

### Creating a Polymer Component
First of all, we're going to create a new component **by duplicating an already existing one**.

In Atom:
* Choose a component and duplicate it's containing folder. That's **the folder src/frontproyecto-app**. Name it visor-curso.
* Name the file inside the new folder visor-curso.html.

Then, in visor-curso.html, change (top to bottom):
* id in dom-module to id="visor-curso"
* class name to VisorCurso.
* Return value of is function to 'visor-curso'.
* Last line of script should be: window.customElements.define(VisorCurso.is, VisorCurso);
.

We should follow an **iterative and incremental** development process, so now we'll **stop here and check if the duplication worked correctly**.
To do this:
* Add visor-curso to the main index.html (remember: **do the import** and **add the custom element <visor-curso>**).
* Check it shows correctly. **You should see the same message twice**.

Nice!, we've just created our first Polymer component on our own. Now **let's tailor it a bit to our needs**.


First, change the template, like so:

```html
<!-- visor-curso.html -->
<template>
  <style>
    :host {
      display: block;
    }
  </style>
  <h2>This is a [[courseSubject]] course</h2>
  <h2>and it's called [[courseName]]</h2>
  <h3>The course is [[courseDuration]] hours long</h3>
</template>
```

If you did the previous lab you'll have noticed we're **reusing code** and adding a bit more. This is a natural process during development: maybe you **identify chunks of code that could be reused** and then **create an abstraction**. The process with Web Components is no different: **identify a part of a website that could be reused**, and **create a Web Component to do it!**.

Now let's add the code for **creating the properties**.

```javascript
// visor-curso.html
class VisorCurso extends Polymer.Element {
  static get is() { return 'visor-curso'; }
  static get properties() {
    return {
      courseSubject: {
        type: String
      }, courseName: {
        type: String
      }, courseDuration: {
        type: Number
      }
    };
  }
}
```

And then, let's use the component. Go to index.html and **give values to the properties**.

```html
<!-- index.html -->
<visor-curso courseSubject="Polymer" courseName="Web Components and Polymer" courseDuration="1"></visor-curso>
```

And **reload the page**.

But, **it's not working!**, why?. When we've named the properties, **we've used camelCase**. Then when we added the property name when including the component, such as in courseSubject="Polymer", **we're using caps in an HTML attribute name**, HTML doesn't like that, and **fails silently**.

To fix this, we have two options:
* Change the property name to something different, not using camelCase but only lowercase or snake_case.
* Keep the camelCase, but use dash-case in the property name when including the component. **Polymer will correctly map the attribute name in dash-case to it's corresponding property in camelCase**.

```html
<!-- index.html -->
<!-- This is the second option -->
<visor-curso course-subject="Polymer" course-name="Web Components and Polymer" course-duration="1"></visor-curso>
```

Try this, it'll work!. Just **keep this in mind because it can lead to hard to spot errors**.

Now let's add two more courses, just to see how easy is to reuse this component we've just created.

```html
<!-- index.html -->
<visor-curso course-subject="Polymer" course-name="Web Components and Polymer" course-duration="1"></visor-curso>
<visor-curso course-subject="Cells" course-name="Cells Specialized" course-duration="2"></visor-curso>
<visor-curso course-subject="HTML" course-name="HTML 101" course-duration="3"></visor-curso>
```

As you can see, it's easy to reuse a component. Of course, this is a basic example, but **with proper abstraction and design**, this principle **can be used in much more complex components to make them reusable**.

And that's it for this lab, good job!. In the next one we're going to **create a component that gets data from an API**, see you there!.
