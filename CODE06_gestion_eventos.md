# Event management
All the components we've made so far are functional, but they're missing something: **communication between them**.

The whole idea of Web Components is to **reuse** them. To make this possible, components **need to communicate both to the outside and the inside**, receiving data from their context and sending data outside.

Think about a conventional web app: a login component may pass to a "dashboard" component **data of the user that just logged in**; the dashboard in turn can pass data from the user profile to other modules, **each one a web component itself**, that will load their own data, and so on. You might think that **this could be done using routing**, and indeed that's correct, but how could we do this, **when all the involved components are on the same page** while **keeping them isolated?**. That's where **events** come in.

Outward communication is done **by events** while inward is done by **changing the value of properties**.
Although **it's possible to call a method inside of a component** directly from outside, it's **not considered good practice** and thus it's **not recommended**.

### Passing data between components
Now we've got the theory covered let's start coding!.

Our goal is to **send data from one component to another**. To do this we need:

* An origin for the data, that's going to be a **component named emisor-evento**.
* A component that receives the data and does something with it, **receptor-evento**.
* A component that **receives data from emisor-evento and relays it to receptor-evento**, **gestor-evento**.

Let's get started and we'll dive deeper into each component as we code it.

###### Event emitter

First of all we need a component that **sends data outside** or, more specifically, **fires an event**.

Create a new component, you can use the duplication method, and name it **emisor-evento**.

```html
<link rel="import" href="../../bower_components/polymer/polymer-element.html">

<dom-module id="emisor-evento">
  <template>
    <style>
      :host {
        display: block;
      }
    </style>
    <h2>Soy el emisor</h2>
    <button on-click="sendEvent">No pulsar</button>

  </template>
```

```javascript
// <script> Don't forget to enclose between script tags!
class EmisorEvento extends Polymer.Element {
  static get is() { return 'emisor-evento'; }
  static get properties() {
    return {
    };
  }

  sendEvent(e) {
    console.log("Pulsado el botón");
    console.log(e);

    this.dispatchEvent(
      new CustomEvent(
        "myevent",
        {
          "detail" : {
            "course" : "TechU",
            "year" : 2018
          }
        }
      )
    );
  }
}

window.customElements.define(EmisorEvento.is, EmisorEvento);
```

In this component:

* When the button is clicked, the **sendEvent** function is called.
* Inside **sendEvent** we log the event that made the call. This is interesting because the **type of e is different depending on the firing event** and **each type has different properties**.
* We also fire a new **CustomEvent event**, that we name **myevent**.
* This **myevent** has **data associated** in the **detail** field. This data is **an object that can contain anything we deem appropriate**. This data will go **outside the component** and other components **can receive it by listening to the "myevent" event**.

Now, following our iterative and incremental approach, let's first **test emisor-evento is working on it's own**.

To do this:
* **Import** it in index.html.
* **Add** it to the body, check that it works.

###### Event manager

With emisor-evento already done, let's continue by adding **gestor-evento**.

This component is going to be -logically- **between the emitter and the receiver**, thus avoiding **tight coupling** between both components and leaving them just loosely coupled (although coupling could be removed almost altogether by using redux, that's outside the scope of this Codelab).

Gestor evento will be a **mediator** between emisor and receptor, **getting the data from the event fired by emisor and sending it to receptor**.

As always, we start by creating the component **gestor-evento**, code:

```html
<link rel="import" href="../../bower_components/polymer/polymer-element.html">
<link rel="import" href="../emisor-evento/emisor-evento.html">

<dom-module id="gestor-evento">
  <template>
    <style>
      :host {
        display: block;
      }
    </style>
    <h1>Yo soy tu padre</h1>
    <emisor-evento  on-myevent="processEvent"></emisor-evento>
  </template>
```

```javascript
// <script> Don't forget to enclose between script tags!
class GestorEvento extends Polymer.Element {
  static get is() { return 'gestor-evento'; }
  static get properties() {
    return {
    };
  }

  processEvent(e) {
    console.log("Capturado evento del emisor");
    console.log(e.detail);
  }
}

window.customElements.define(GestorEvento.is, GestorEvento);
```

Let's see what's happening here:

* As you can see **we have emisor-evento inside gestor-evento**, that way we can **listen to events that emisor-evento fires**.
* We chose to listen to the **myevent event using on-myevent="processEvent"**.
* The handler function for the event, processEvent, just **prints the data inside the captured event**.

Now **remove emisor-evento from the body in index.html** and import and add **gestor-evento** to see both components are linked correctly and working.


###### Event receiver

Now for the **last step** in the process: emisor-evento has fired an event, it has been captured by gestor-evento, and now gestor-evento is going to **send the event's data to receptor-evento**.

Create the component **receptor-evento**.

```html
<link rel="import" href="../../bower_components/polymer/polymer-element.html">

<dom-module id="receptor-evento">
  <template>
    <style>
      :host {
        display: block;
      }
    </style>
    <h2>Soy el receptor</h2>
    <h3>Este curso es [[course]]</h3>
    <h3>y estamos en el año [[year]]</h3>
  </template>
```

```javascript
// <script> Don't forget to enclose between script tags!
class ReceptorEvento extends Polymer.Element {
  static get is() { return 'receptor-evento'; }
  static get properties() {
    return {
      course: {
        type: String
      }, year: {
        type: String
      }
    };
  }
}

window.customElements.define(ReceptorEvento.is, ReceptorEvento);
```

Receptor-evento has nothing new: **it's a component with a couple of properties** that are **bound two-way in the template**. What's new however is how we're going to **set the value of those properties**.

First **import receptor-evento in gestor-evento and add it to the template**.

```html
<!-- gestor-evento.html -->
<link rel="import" href="../receptor-evento/receptor-evento.html">

<h1>Yo soy tu padre</h1>
<emisor-evento on-myevent="processEvent"></emisor-evento>
<receptor-evento id="receiver"></receptor-evento>
```

Don't forget to **add an id to receptor-evento**.

Before carrying on, stop here and check **receptor-evento shows correctly** together with gestor and emisor.

Now, using the id, we'll **assign the value of the data received from the event fired by emisor-evento to the properties bound in receptor-evento**.


```javascript
// Gestor evento Javascript
processEvent(e) {
  console.log("Capturado evento del emisor");
  console.log(e.detail);
  this.$.receiver.course = e.detail.course;
  this.$.receiver.year = e.detail.year;
}
```

And this is it! If we got it all right, **when we click the button in emisor-evento, data will travel and show up in receptor-evento!**, completing the data flow.

With this in our toolbox, now we can refine it as much as needed. In real world applications, there are going to be **a number of components in any given page, listening to one or more events each one, and maybe those events come from different components**. Managing the flow of data is key to keeping components as loosely coupled from each other as possible and having our app working as planned, with data travelling between components as we need.

And this concludes this lab!. See you at the next where we'll learn about **routing!**.
