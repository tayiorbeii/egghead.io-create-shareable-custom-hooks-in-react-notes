# 01. Change a Stateful React Class Component to a Function Component with the State Hook
[Lesson Link](https://egghead.io/lessons/react-change-a-stateful-react-class-component-to-a-function-component-with-the-state-hook)

We start with a class component of a feedback text area that we can submit to an API that console logs our text.

Create a new file `Feedback.function.js`, and paste our original class component code.

At the top of the file, add a destructured `useState` import from React:

```js
import React, { useState } from 'react'
```

Next we'll change our class component declaration to export a function:

### Component Declaration Before:
```js
export class FeedbackClassComponent extends React.Component {
  ...
```

### Component Declaration After:
```js
export function FeedbackFunctionComponent() {
  ...
```


Next thing we'll do, we'll take a look...this is how State is declared in a class component. We're going to delete that and instead use the State Hook. We do that by declaring an array which returns two elements, the first being the actual value, in this case text, and then a function normally called SET and then the name of the value.

Now that we've started converting our component to be a function, we need to replace the class way of adding state with our state hook instead.

On the line after our function declaration, we'll add this line:
```js
export function FeedbackFunctionComponent() {
  const [text, setText] = useState('')
  ...
```

What we are doing here is creating two new variables `text` and `setText` that are being destructured from an array, that is being returned from a call to `useState` where we are passing in an intial value of an empty string. `text` is our text, and `setText` is a function that updates our `text` variable.

### Refactoring Class Methods
We'll start by refactoring `handleSubmit`. 

Add the function keyword, and change `this.state.text` to just `text`. We'll also replace `this.setState` with a call to `setText('')` and pass in an empty string to reset our text value.

### `handleSubmit` Before
```js
handleSubmit(e) {
  e.preventDefault()
  console.log(`Submitting API response to API: ${this.state.text}`)
  this.setState({
    text: ''
  })
}
```

### `handleSubmit` After
```js
function handleSubmit(e) {
  e.preventDefault()
  console.log(`Submitting API response to API: ${text}`)
  setText('')
}
```

We'll follow a similar approach for `handleTextChange`:

### `handleTextChange` Before
```js
handleTextChange(e) {
  const updatedText = e.target.value

  this.setState({text: updatedText})
}
```

### `handleTextChange` After
```js
function handleTextChange(e) {
  const updatedText = e.target.value

  setText(updatedText)
}
```

The `render()` method is no longer needed–- since we are using a function component, we can just return the JSX that we want to render.

We will also remove "`this.`" from the `<Form>` component's `onSubmit` handler, the `<Textarea>`'s `value` prop & `onChange` handler:
```js
// inside the `return`
<Form onSubmit={e => handleSubmit(e)}>
  ...
  <Textarea
    value={text}
    onChange={e => handleTextChange(e)}
  />
```

## Updating `App`

Inside of `App.js`, we'll add a new import for our new `FeedbackFunctionComponent`...
```js
import { FeedbackFunctionComponent } from './components/Feedback/Feedback.function' 
```
... and then update our `<InnerContainer>` to point at it.

