CREATE A PERSON VISOR
==========================================================

So far we've seen how to show data in a Polymer component **getting that data locally**. But that doesn't cover all scenarios.
More often than not, we'll need to get data **from an external resource**, such as a REST API.
Our goal now is to create a Web Component that can **get data from a REST API and print it**.

###### Duplicating a component
First of all, we're going to create a new component, name it visor-persona, **by duplicating an existing one** as we've done previously.

Here's the process step by step as a reminder.

In Atom:
* Choose a component and duplicate it's containing folder. Name it visor-persona.
* Name the file inside visor-persona.html.

Then, in visor-persona.html, change:
* id in dom-module to id="visor-persona"
* class name to VisorPersona.
* is function to is VisorPersona.is.
* Return value of is function to 'visor-persona'.
* Last line of script should be: window.customElements.define(VisorPersona.is, VisorPersona)
.

Stop here and **check if the duplication worked correctly**. Remember: import the component and include it in index.html.

If everything is OK, let's **add the properties of a person**. You may be -correctly- wondering how do we know in advance which data to show, **truth is we don't know until we check it directly**, be it with directly the source or API documentation.

Let's do that right now so **we can be 100% sure** of which properties to show.

Open **Postman** and **send a GET request** to this URL https://services.odata.org/V4/OData/OData.svc/Persons .
There we have it! A **collection of persons**.
We're going to focus only **on the id and name** of each person.

Let's **add those properties**.

```javascript
// visor-persona.html
class VisorPersona extends Polymer.Element {
  static get is() { return 'visor-persona'; }
  static get properties() {
    return {
      name: {
        type: String
      }, id: {
        type: Number
      }
    };
  }
}
```

And let's **print those properties in the template**.

```html
<!-- visor-persona.html -->
<!-- links -->
<dom-module id="visor-persona">
  <template>
    <!-- style  -->
    <h2>Mi id es [[id]]</h2>
    <h3>y me llamo [[name]]</h3>
  </template>
```

###### Getting data from the API

We've already laid down the structure for presenting data, so now **what we need to do is actually getting it!**.

To do this we'll use **AJAX** (remember it stands for **Asynchronous Javascript and XML**) For using AJAX with
Polymer we need **iron-ajax**.

Install it by entering this on a terminal, **inside the Polymer app root folder**.

    bower install iron-ajax --save

And **import and add iron-ajax to the template**.
```html
<!-- visor-persona.html -->
<link rel="import"
  href="../../bower_components/iron-ajax/iron-ajax.html" />

<!-- In dom-module -->
<h2>Mi id es [[id]]</h2>
<h3>y me llamo [[name]]</h3>

<!-- Place iron-ajax below the HTML -->
<iron-ajax auto
  id="getPerson"
  url="https://services.odata.org/V4/OData/OData.svc/Persons"
  handle-as="json"
  on-response="showData"
>
```

In this code:

* auto means **send the request automatically** without us needing to send it explicitly.
* url points where to send the request.
* handle-as **directly parses the response to JSON** (use only when the API returns JSON).
* on-response indicates **which function should be called when the response arrives from the server**.

Now let's **add the handler function to JS**.

```javascript
// visor-persona.html
class VisorPersona extends Polymer.Element {
  //static functions

  // After the properties, but inside the class.
  showData(data) {
    console.log("showData");
    console.log(data.detail.response);
  }
}// Closes the class
```

Right now, the component should look like this.

```html
<link rel="import" href="../../bower_components/polymer/polymer-element.html">
<link rel="import" href="../../bower_components/iron-ajax/iron-ajax.html">

<dom-module id="visor-persona">
  <template>
    <style>
      :host {
        display: block;
      }
    </style>

    <h2>Mi id es [[id]]</h2>
    <h3>y me llamo [[name]]</h3>

    <iron-ajax auto
      id="getPerson"
      url="https://services.odata.org/V4/OData/OData.svc/Persons"
      handle-as="json"
      on-response="showData"
    >
  </template>
```

```javascript
<script>
    /**
     * @customElement
     * @polymer
     */
    class VisorPersona extends Polymer.Element {
      static get is() { return 'visor-persona'; }
      static get properties() {
        return {
          name: {
            type: String
          }, id: {
            type: Number
          }
        };
      }

      showData(data) {
        console.log("showData");
        console.log(data.detail.response);
      }
    }

    window.customElements.define(VisorPersona.is, VisorPersona);
  </script>
</dom-module>
```

Load this on the browser and let's stop here and check we're on the right track.

We should see:
* The template but no values. That is: **Mi id es** and, a line below **y me llamo**.
* If we **check the console** (F12 in the browser and then click "Console" in Chrome), we should see **two logged items**:
* The first is **the name of the handler function**, showData.
* The second should be **an object containing the REST API response**. You should see a **small arrow on the left of the object**. Click it to expand it, then click again in "value" and there we have them! **an array with our persons**.

OK, so next we need to **print the data in our template**. To do this we need **to assign the value of the properties to a person**. For the sake of simplicity **we'll show the data only for the first person**.

```javascript
// visor-usuario.html
class VisorPersona extends Polymer.Element {
//static functions

// After the properties, but inside the class.
  showData(data) {
    console.log("showData");
    console.log(data.detail.response);

    this.id = data.detail.response.value[0].ID;
    this.name = data.detail.response.value[0].Name;
  }
}// Closes the class.
```

And there we go! Check it in the browser and, if everything is correct, you should **see the user data in the template**. **Congratulations!**.

It's important to note that, as you can see in the right hand part of the assignments, **our property names are different than those of the Person**. We have **name** but the API has **Name** for the same value. **Never** assume anything regarding property naming, **always** check directly with the data source to make sure you're **getting the data with the right property name**, as this can, and will, often lead to mistakes.
