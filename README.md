# Part 1: The Mystery :mag:

This is a _mystery_ lab! That means that solving one task will lead to solving the next task, and the end result will be a **surprise**! Good luck!

## Task 0: Create a Gatsby site

The rest of this lab assumes that you have a Gatsby site set up and that you are in that project directory. Run `gatsby new [the path you want to create the new project]` to get started.

After that, use `gatsby develop` to start serving your project from a development server.

:checkered_flag: **Finish:** Make sure you can see the default Gatsby page at `localhost:8000`.

## Task 1: A React refresher

### Building Components :hammer: 

The first exercise for this task will be a walkthrough for building a React component, as a review of the last couple weeks' React content. Then, I'll provide you a high-level specification for what other components need to be built, and you will build them!

**Walkthrough: Contact Form `<ContactForm />`**

The walkthrough will take you through building a common feature you'll be implementing in React sites: forms. There are libraries to reduce some of the complexity that this walkthrough delves into, but it's important to understand how it works to take advantage of React's component methodology and lifecycle hooks. 

Let's start with a rough wireframe of how we want our contact form to look:

```
          ------------------
Name     |                  |
          ------------------
          ------------------
Email    |                  |
          ------------------
          ------------------
Message  |                  |
         |                  |
         |                  |
          ------------------
                *----------*
                |  Submit  |
                *----------*
```

---

:question: **Answer before continuing:** Where in the React component that renders this form would we store _the input of the user_ (name, email, message)?

:one: `this.state`
:two: `this.props`
:three:  `window`

---

If you answered **state**, you'd be correct! Here's why:

- **State** is for managing this _current component's_ data at a certain point in time. 
- **Props**, on the other hand, is for passing this current component data from a _parent_ component.
- We never want to store data on the global namespace (`window`).

Let's start off with a basic React component. You can create this in `src/pages/contact.js` - _recall that all components exported within the `src/pages` directory also become routes!_ In other words, this new component at `src/pages/contact.js` will be served at `/contact` on your website.

> **Note:** This might seem like an odd suggestion, but _don't_ copy and paste code from here to your files! Type it out - make sure you understand what's going on.

**`src/pages/contacts.js`**
```jsx
import React from 'react';

class ContactForm extends React.Component {
    
}
    
export default ContactForm;
```

The beginning of our contact form! Only a few things are happening here:

- Importing React - required for _all_ React components (even if you're not using the `class` syntax!
- Creating a class called `ContactForm` that inherits from React's `Component` class.
- Exporting our new class as the `default` export.
    
> <sub>**Hi, this is an optional sub-note. You can ignore this!** Curious as to what this `default` thing is? This is what allows us to import components from others as `import ContactForm from './ContactForm'` - this is actually identical to `import Duck from './ContactForm'` because it looks for the `default` export, not the name. [More information at MDN,](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) for the interested.</sub>


### :pencil: Adding form elements 

Let's now add our text boxes and labels for the form into this component. Remember that to _display_ things from our React component, we need a `render` method implemented:

**`src/pages/contacts.js`**
```jsx
render() {
  return (
    <form>
      <label for="name">Name</label>
      <input type="text" id="name" />
    </form>
  );
}
```

Let's break down what's happening here, too:

- We created a `render` method, which is called every time React sees that this component's `props` or `state` has changed. This method tells us how to display this component in your browser.
- We rendered a `<form>` that includes a single `<label>` and `<input>` HTML element. 

**Navigate to `localhost:8000/contact` to make sure this shows up!**

Nothing too exciting just yet - and not from React's perspective either.

Notice that we have nothing here that would inform React of what the _contents_ of this `<input>` are - we can't tell what the user typed in.

**Before you continue,** create two more sets of `<label>` and text box elements for Email and Message, as in our wireframe above. Set your `id` and `for` attributes appropriately!

> **A Handy Hint :tm:**: Use a `<textarea>` element instead of an `<input>` if you want a larger text box. `<textarea>` elements don't need a `type` attribute!


### :boom: Using event handlers

> **Before starting this section,** you should have a complete form as represented in the wireframe at `/contact` of your Gatsby site.

To now inform React of changes to our text boxes, we need to set "event handlers" on our `<input>` and `<textarea>` elements.

What does this mean? - it means that we need a **function** that responds to events emitted by these elements and is able to get some information about them (like their contents!). In this case, we are responding to an event called `onchange`, which is emitted by elements that take user input every time they are changed by a user. Let's start by defining one for our Name text box:

**`src/pages/contacts.js`**
```jsx
class ... {
  onChangeEmail(event) {
    console.log(event.target.value);
  }
  
  render() {
    return (
      ...
      // This is NOT a new input - change your
      // existing Name text box and add this onChange
      // prop.
      <input
        type="text"
        id="name"
        onChange={(event) => this.onChangeEmail(event)}
      />
      ...
      // You should have more than this input
      // here, but it is omitted to save space!
    );
  }
}

export ...
```

Let's talk about what's happening again here:

- We define a function called `onChangeEmail` within the `ContactForm` component. It takes a single argument called `event` that represents the [Event object](https://developer.mozilla.org/en-US/docs/Web/API/Event) that we receive from the emitted `onchange` event referenced above.
- `onChangeEmail` was defined to just log the value of `event.target.value`, which represents the current value of the `<input>` element that emitted this event.
- We added a prop called `onChange` to the `input` element (our Name text box). We passed in a **_function_** that takes an `event` parameter and passes it to the `onChangeEmail` function we defined.


Now, open DevTools in Chrome (by right-clicking and clicking "Inspect Element" or keying `Ctrl [Windows] / Command [Mac] -Shift - J`). Click into the Console tab if you aren't already in it.

Try typing into your Name text box, and see that what you typed in is logging inside the Console. We now see that React is being informed of changes to our Name text box!

**Before continuing, create event handlers for all of your text boxes.** You should see the value of each box log to the console.

### :aerial_tramway: Storing input values

We now have event handlers on our text boxes, which means we get updated every time our text boxes gets typed into. That's a great first step, but now, we need to do something beyond logging to the console. We need to _store_ the values of our inputs within the `state` of the `ContactForm` component (_remember the question asked at the beginning?_).

This is how we'll store it inside of the `state` object:
```javascript
// this.state is:
{
  name: "John Denero",
  email: "denero@berkeley.edu",
  message: "How do I join Web Team?"
}
```

To do this, change the implementation of your `onChangeX` functions to look like:

**`src/pages/contacts.js`**

```jsx
onChangeEmail(event) {
  this.setState({
    email: event.target.value
  });
}
```

Notice here that we're not directly modifying `this.state = [something]`! To change state from **_anywhere_** in our component (except for the `constructor`, which we're not using), we need to call `setState` so that the component knows to re-render using a new version of `state`.

Verify that `state` inside of your `ContactForm` is updating while you type by inspecting with the React tab inside of DevTools (don't have it? install [here](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)). 

**Hint:** you can use the eyedropper tool (shown below) to click on the `ContactForm` and see the `state` object in the sidebar:

![](https://cl.ly/1L2B3M04062k/Image%202018-03-13%20at%201.48.48%20PM.png)

**One last thing**: you might have thought that our `onChange` functions look awfully similar to define it for every one of our inputs. We can generalize this by using the ID of the `event.target` as the key to update in state! That looks like this:

```jsx
onChange(event) {
  this.setState({
    [event.target.id]: event.target.value
  });
}
```

Don't forget to update the `onChange` props of your `<input>`s and `<textarea>` if you decide to do this!


### :cloud: Sending form contents

One last thing to make our form functional: actually allowing it to be submitted!

To accomplish this, first add a submit button within your form:

```jsx
<input type="submit" value="Send message" />
```

We will use another event handler to respond to the `onsubmit` event of our `<form>` element. Define that as follows:

**`src/pages/contacts.js`**
```jsx
class ... {
  
  onSubmit(event) {
    event.preventDefault();
    fetch('https://untitled-7u6istdb7h9e.runkit.sh/', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(this.state)
    })
    .then(response => {
      if (!response.ok) throw new Error(response);
      return response.json());
    .then((response) => {
      console.log(response);
      alert('Sent successfully!');
    })
    .catch(err => console.error(err));
  }
  
  ...
}

export ...
```

This is a lot of new code. Let's break down what we've done here:

- We've created a method inside of the `ContactForm` component called `onSubmit`. We intend it to be an event handler (for the `onsubmit` event), so we have it take an event object in parameters.
- We call `event.preventDefault()` to change the default behavior of what `onsubmit` causes. Try removing this line. What happens?
- In the method we define, we use `fetch` to send an external `POST` request to some endpoint (I defined the behavior of this endpoint beforehand).
- Within this request, we send a String representation of `this.state` (_what is in `this.state`_? Your form contents!)
- If the request succeeds, we parse the response as a JavaScript object with `response.json()` and then we log this object to the console and show an alert dialog to the user (saying "Sent successfully!").
- If the request fails, we also log the failure response to the console.

The next step is to call your `onSubmit` function in the `onSubmit` prop of your `<form>` element. Take a look at how we did that with `<input>`s and `onChange` if you're stuck. 

**Finally, make sure that your request works!** You should be able to fill out a form, submit it, and see the response log successfully to the console. The response should be a mystery.

:mag: **Before you finish,** follow the mystery!
**_This is required to complete the rest of this lab._**

Place the mystery inside of the root of your project directory.

## Task 2: Pulling in dynamic data

Let's switch gears a little bit. Now, we want to use what we've learned about GraphQL to pull in data from a _mysterious_ source (Google Sheets) and render it in our site.

Begin by running `npm install --save gatsby-source-google-sheets` in your terminal (you may have to open another window if your Gatsby server is still active).

Edit `gatsby-config.js` to include this object in the `plugins` array:

```javascript
plugins: [
  'gatsby-plugin-react-helmet',
  // add this object:
  {
      resolve: 'gatsby-source-google-sheets',
      options: {
          spreadsheetId: '1J9tJQNVYoN64QbZjOMwpSmA62GWOehcUpgVeTDv6jNA',
          worksheetTitle: 'Sheet1',
          credentials: require("./InnoD\ Mystery-975d82e0c439.json")
      }
  }
]
```

Restart your Gatsby server (Ctrl-C will exit the running process) and head to `localhost:8000/___graphql` to access GraphiQL, an interface for browsing GraphQL data.

On the left is a box for typing in queries and getting responses back. Try writing one - here's a hint: Start with the following:

```jsx
{
  googleSheetSheet1Row {
    name
    email
    message
  }
}
```
and play with it from here!

_... to be continued_
