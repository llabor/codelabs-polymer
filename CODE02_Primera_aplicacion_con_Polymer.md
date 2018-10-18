# Web Components and Polymer, an approach

So, here we are! Ready to start developing with Polymer, are we? or maybe not?.

**Before we start tinkering with Polymer** we need to understand it's foundations: **Web Components**.

## Web Components

So, **what are Web Components?**.

If we think of a component in **software engineering and object oriented programming**, we may picture a part of a subsystem, right?. That is: a, relatively small, **piece of software that performs a given task**.
If we go to a higher level of abstraction, we'll see that these components are, if well designed and coded, **independent** and **reusable**. These two properties are **very closely linked** to **low coupling** and **high cohesion** both well known properties of well designed software.
A component being **independent** is enabled by **encapsulation**; **reusability** is enabled by **good abstraction** in the design and any type of **inheritance** and/or **polymorphism** in the programming language of choice.

So far so good but, **what about the Web**?. Take a component to a web page, **we may see a widget** right?. But the widget itself is just HTML, or a library, or HTML and Javascript for added functionality, but **it's not an intrinsic part of the Web**. How to change that? the answer is **Web Components**.

With Web Components, in essence, what we're doing is **taking those well known software engineering good practices to front-end development**, to the Web.

The main difference is that, in the Web, we're not going to use a "traditional" programming language, such as Java, PHP, C, for coding, we're going to use web technologies!. These we know well: **HTML, Javascript and CSS**.

But that's not all!. To build Web Components, we'll use the aforementioned technologies, but we also have **Web Standards** that "enable" the web to **provide what we need for Web Components to be really reusable and independent**.

These are:

* **Custom elements** - Using Custom Elements allows us to use HTML tags **that are not part of the HTML standard**. We can **create our own tags** and **add them to HTML** as if they were standard tags!.

* **HTML Import** - This allows us to **reuse HTML** by importing components into any page.

* **HTML Template** - The use of templates allows for the **creation of "chunks" of code** that can be **reused in any number of web pages**.

* **Shadow DOM** - This one is usually hard to visualize, but **it's instrumental for proper encapsulation of the components**. All the Web Components we add to a page have their own DOM, that is, a DOM **inside** the page's own DOM. This DOM is called **shadow DOM** and isolates the component from the rest of the HTML, **allowing it to have it's own html and styles**.

So, start with the ingredients of the modern web, combine them with these four standards and the outcome is Web Components!.

### Polymer

So now we understand Web Components but, **what's Polymer and where does it fit?**.

**Polymer** is a **library** that makes development of modern web applications that use Web Components easier.

We already have a **good basic knowledge of Web Components**, so to learn about how Polymer works, it's best that we step right into it and build **our first Polymer app!**.

###### First Polymer App

Open a Terminal. If you haven't already, **create a folder to store all the course's work**. You can name it **projects**.

Inside that folder, create another to store our first polymer-app. You can choose any name you want. I named it **polymer_test**.

Go inside the folder you just created. **Now we're going to create the app**. Enter this command:

    polymer init

Choose the option to create a **polymer application**.

As for the rest of options:

* Application name: **polymertest**
* Main element name: **polymertest-app**
* Brief description of the application: **Anything you want**.

Now you should see a lot of output, **this is because other components are being downloaded**.

Once it's finished, **let's check the result!**. If you chose Atom as your editor, just enter this in the command line:

    atom .

**This will open the whole folder in atom for edition**. This is our very first Polymer app!. This is what we need to know about the files on it:

* **bower.json** - Same as package.json, this is the file that has all the dependencies our app needs.
* **bower_components** - This folder is the physical storage for the dependencies included in bower.json.
* **src** - This folder contains the components of the app.

The **home** of our app is the **index.html** file. Let's open it in Atom!.

The file is a "normal" html file but, if you look carefully, **you'll notice it uses two of the standards we mentioned before: HTML Import and Custom Elements**.

This line:

```html
<link rel="import" href="/src/polymertest-app/polymertest-app.html">
```

Is **importing** a component so we can use it in this page.

And this other line:

```html
<polymertest-app></polymertest-app>
```

Is actually **including the component in the page** using a html tag that is not part of the standard, that is, a **custom element**.

Now all that's left is **seeing the result!**. For this, just go open a terminal **in the folder that contains the project** and enter:

    polymer serve --open

This will **start a server and open a browser in the index page**. You should see a white page with a Hello yourappnamehere message.

You may wonder where is that message coming from? We'll learn that and much more in the next codelab, where **we'll study the structure of a Polymer component!**.
