In the previous codelab we created our first Polymer app using polymer-cli and took a look at it's structure.

Let's **start from there and dig deeper!**. Remember the message we got when we started the app?. If you don't have the server running already, **open a terminal at the polymer app root folder** and enter:

    polymer serve --open

By looking at the index.html file, we can see we have a custom element in the html body, but **we don't see any html that looks like the message we have in the home page**. This is because that HTML is **coming from the custom element itself**, more precisely **from it's template**. If you recall the first codelab, **HTML Template was one of the standards Web Components, and thus Polymer, is built on**.

Let's learn more about the component itself.

## Structure of a Polymer Component

In the folder containing our app, there's a **src** folder. This is the place **where the components are placed by default**. Open it and you'll see a folder named as you named your app, mine is **polymertest-app**. This is **one component**. Inside that folder it's the html file **that contains the component itself**. Open it and let's see what's inside.

* Style (CSS) - inside the template, inside the **style tag**.
* Template (HTML) - inside the template, outside the **style tag**.
* Script - (JS) methods and properties, outside the template, inside the **script tag**.

This is the basic outline, but there're plenty of other things going on:

* In the first line **the Polymer element is imported**, this is the **base element** from which all other Polymer elements inherit from.
* The bracket "[[]]" operators are used for "printing" the value of a property. This is known as **data binding** (more on this soon).
* In the script part there's **a class**, similar to traditional object oriented programming, a **class is declared and it inherits from Polymer.element**, the base element.
* Uses an static method to store the custom element name, this is **what we need to write between "<>" to include the element in any page**, after having imported it.
* Uses an static method to store the component's properties. These we'll use for **managing the state of the component** and **print their value when needed**.
* In the last line is where the custom element name is linked to its implementation (the class).


### Working with a Polymer Component

Now we know how a Polymer Component is structured, **let's start tinkering with it!**.

First of all we're going to add some HTML **outside** the component. This way **we can see it's boundaries**.

Go the main index.html at the polymer app root folder and add:

```html
<!-- index.html-->
<body>
  <polymertest-app></polymertest-app>
  <h3>This is not Polymer</h3>
</body>
```

**Make 100% sure you save the changes** and reload the page in the browser. You should see an h3 **just below the hello message**. This h3 is **outside the component**, so you can see it's easy to combine custom elements and standard elements.

## Data Binding
**Data binding** is the process by which we can "link" **bind** a **property from the custom element to an attribute or element on it's local DOM**.

We've already seen an example of it. If you go back to polymertest-app you'll see that the value of prop1 is **bound** to the local DOM by using **[[prop1]]**. This is called **one way data binding**, let's see how it works.

###### One Way Data Binding
Data binding can work One Way, using **[[]]**. This means
**the value of the property "flows" from the property
itself to its linked elements**, but not the other way round.

We can easily test this. Go to polymertest-app and **change the value of prop1**:

```html
<!-- polymertest-app.html-->
<script>
    /**
     * @customElement
     * @polymer
     */
class PolymertestApp extends Polymer.Element {
  static get is() { return 'polymertest-app'; }
  static get properties() {        return {
      prop1: {
        type: String,
        // Change this!
        value: 'World!'
      }
    };
  }
}
</script>
```

Reload and **see the value change!**. The data is flowing **from the property to the element**.

Values can be set when adding the custom element too.

```html
<!-- index.html-->
<body>
  <polymertest-app prop1="Test"></polymertest-app>
  <h3>This is not Polymer</h3>
</body>
```

Reload and see the value being overwritten!.

Any number or properties can be added to any component. First we **add the target elements in the local DOM**.

```html
<!-- polymertest-app.html -->
<h2>This is a [[courseSubject]] course</h2>
<h2>and it's called [[courseName]]</h2>
```

And add the properties in the appropriate function.

```javascript
// polymertest-app.html
class PolymertestApp extends Polymer.Element {
  static get is() { return 'polymertest-app'; }
  static get properties() {
    return {
      courseSubject: {
        type: String,
        value: 'Polymer'
      }, courseName: {
        type: String,
        value: 'Web Components and Polymer'
      }
    };
  }
}
```

Reload and check the result!.

###### Two Way Data Binding
Data binding can work in Two Way mode too, using **{{}}**. This means the value of the property **"flows" from the property itself to its bound elements** but **from any bound element to the property too**.

We'll see it clearly with an example. Let's add an input to the local DOM of our component and bind it using two way syntax.

```html
<!-- polymertest-app.html -->
<input type="text" value="{{courseName}}" />
```

This **should** work, **but it doesn't**. The reason is somewhat complex: Polymer achieves binding an element two ways by **listening to an event fired when the value of the property changes**. Say we have a property named **test**. When the value of test changes, by default the event **test-changed** fires, Polymer listens to it and changes the value accordingly; **but**, because input is a **native** -non Polymer-- **element**, the event is not named as the convention but **input**, so we need to **explicitly tell Polymer which event to listen to**. This is done like so:

```html
<!-- polymertest-app.html -->
<input type="text" value="{{courseName::input}}" />
```

This, when the input value changes, **will listen to the input event**, and will change the value of the property as expected. You can test it by reloading the page, and **entering any course name in the input** value.

And that's it for this lab! See you at the next, where **we'll create a brand new Polymer component!**.
