Statify
===

Transparently handle `props` and `state` on ReactJS components.

The problem
---

Let's assume we have a `Textbox` ReactJS component that has some `props`. Let's assume, also, that we have an external `Controller-Component` that is responsible for
handling the view state and rendering. `Controller-Component` renders the `Textbox` component every time it needs the `Textbox` to be different. This problem is very simple and
shouldn't get you into trouble.

Now let's assume you have a slightly more complex component called `Autocomplete`. `Autocomplete` has a button that, when clicked, toggles the state of a dropdown list.

Now you have 2 options and a dilemma:
 - You can let the `Autocomplete` component to be independent and happy. That is... You can let it keep it's `open` state. In this case, the `Controller-Component` will
 not even know then the `Autocomplete` component changed from open to closed or vice-versa. The `Autocomplete` will be master of it's own state and future re-renders
 won't have any effect.
 - You can say: "Look, Autocomplete, I don't want to to keep your own state. I want you to inform me everytime you want to be rendered in the `open` or `closed` state
 and **I** will do it. You tell me when the user clicked the `toggle` button and I will re-render you passing the appropriate value for the `open` `prop`.



