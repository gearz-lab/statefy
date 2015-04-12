Statefy
===

A ReactJS mixin that allows you to transparently handle `props` and `state` on ReactJS components.

Motivation
---

Let's assume we have a `Textbox` ReactJS component that has some `props`. Let's assume, also, that we have an external `Controller-Component` that is responsible for
handling the view state and rendering. `Controller-Component` renders the `Textbox` component every time it needs the `Textbox` to be different. This problem is very simple and
shouldn't get you into trouble.

Now let's assume you have a slightly more complex component called `Autocomplete`. `Autocomplete` has a button that, when clicked, toggles the state of a dropdown list.

Now you have 2 options and a dilemma:

 1. You can let the `Autocomplete` component to be independent and happy. That is... You can let it keep it's `open` state. In this case, the `Controller-Component` will
 not even know then the `Autocomplete` component changed from open to closed or vice-versa. The `Autocomplete` will be master of it's own state and future re-renders
 won't have any effect.
 2. You can say: "Look, Autocomplete, I don't want you to keep your own state. I want you to inform me every time you want to be rendered either in the `open` or `closed` state
 and I will do so. You tell me when the user clicked the `toggle` button and I will re-render you passing the appropriate value for the `open` `prop`". If you
 go that route, for every `prop` you passed to `Autocomplete`, you need a way to be informed that it should change, so you can re-render the component
 passing the new value. If used this way, the `Autocomplete` component doesn't have to keep it's own state. The `Controller-Component` does.

The problem is: When you create and publish component, **you don't know whether the developer using your component will want it to behave as (1) or as (2)**.

The solution
---

Statefy is a mixin that, when added to your ReactJS component, will make it to behave consistently regardless of whether the developer using it prefers (1) or (2).

**Getting the value of a property**

In order to get the value of a property, regardless of it being `prop` or `state`:

    var open = this.get("open");

The `get` function will first try to find a matching property on the `state`. If it doesn't find it, it will try to take it from the `props`.

**Setting the value of a property**

In order to set the value of a property, regardless of if being `prop` or `state`:

    this.set("open", true);
    
When you change a property value, Statefy will trigger an event name dynamically generated according to the property name,
for instance, if the property is called "open", statefy will trigger the "onOpenChange" event, so you could handle it like 
this:

    <Autocomplete
         id="myAutocomplete"
         onOpenChange= {
                 function (e) {
                     // if value has changed
                     e.preventDefault();
                     var newState = React.addons.update(
                         this.state,
                         {autocompleteOpen: {value: {$set: e.value}}});
                     this.setState(newState);
                 }.bind(this)
             }
     />

Once the event is handled, `e.preventDefault` prevents the `Autocomplete` to store the `open` property as an internal
 state. When you use `e.preventDefault`, you are using the component in the approach (2), that is, you, the `Controller-Component`,
 are responsible for re-rendering the `Aucomplete` component because the `open` property changed.
 
If we didn't use the `e.preventDefault`, the `Autocomplete` would store `open` as state and future renders would simply ignore
what would be passed as prop. This would be approach (1).

If the property was called `value`, the event name generated would be simply `onChange`. Example:

     <Textbox
         id="textbox2"
         value={this.state.textbox2.value}
         onChange={
             function (e) {
                 e.preventDefault();
                 var newState = React.addons.update(
                     this.state,
                     {textbox2: {value: {$set: e.value}}});
                 this.setState(newState);
             }.bind(this)
             }
     />
     
You can even assign an event handler to deal with any property change, the event is `onAnyStateChanged`. Example:

     <Textbox
          id="textbox1"
          value={this.state.textbox1.value}
          onAnyChange={
              function (e) {
                  // if value has changed
                  if (e.key == 'value') {
                      e.preventDefault();
                      var newState = React.addons.update(
                          this.state,
                          {textbox1: {value: {$set: e.value}}});
                      this.setState(newState);
                  }
              }.bind(this)
              }
      />
      
Installation
---

If you are using CommonJS:

Install by `npm`:

    npm install statefy
    
Now...

    var statefy = require("statefy");
    const Textbox = React.createClass({
        mixins: [statefy]
    });
    
Or, alternatively, just [download the mixin](https://github.com/gearz-lab/statefy/blob/master/index.js) and use it as described above.
      
License
---

Statefy is provided under the [MIT](https://github.com/gearz-lab/statefy/blob/master/LICENSE) license.