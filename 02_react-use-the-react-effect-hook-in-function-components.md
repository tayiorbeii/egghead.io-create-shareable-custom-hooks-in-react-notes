# 02. Use the React Effect Hook in Function Components
[Lesson Link](https://egghead.io/lessons/react-use-the-react-effect-hook-in-function-components)

Copy the `Feedback.function.js` to a new file called `Feedback.effect.js`.

Add `useEffect` to our destructured imports, and rename our components to `FeedbackEffectComponent`:
```js
import React, { useState, useEffect } from 'react'
```

Now below our line where we have `text` and `setText` being declared, we will add a call to `useEffect()`.

`useEffect` is an imperative function that can return a cleanup function. It accepts two parameters: an effect, which is mandatory, and then an optional deps. Here, deps stands for dependencies, and is an array of any. 

We'll pass an anonymous arrow function that calls `console.log()` into `useEffect()`:

```js
export function FeedbackEffectComponent() {
  const [text, setText] = useState('')
  useEffect(() => {
    console.log('logging from the useEffect hook')
  })
}
```

Add the `FeedbackEffectComponent` import to `App.js` and add it to the list of Feedback components to be rendered on the page.

When we visit the page, we can see our new log statement.

Refresh the page, and then start typing in the `FeedbackEffectComponent` text area. Notice how every time you press a key, the `console.log()` message is displayed in the console.

### Adding a Dependency Array
Let's add an empty array as the second parameter to our `useEffect` call:

```js
useEffect(() => {
  console.log('logging from the useEffect hook')
}, [])
```

Now when we reload our page, our message is logged out when the page loads, but does not log every time we press a key.

The empty dependency array that we added to our `useEffect` call is like us saying to React, "Hey, here is my `useEffect`, here's the function, and here's an array of values I want you to watch in order to know when you should call my effect again."

Now lets add our `text` variable to the dependency array:
```js
useEffect(() => {
  console.log('logging from the useEffect hook')
}, [text])
```

If we go back and we refresh, we should see the console log because of the initial render. React calls our effect, which we see here. Then if we start typing, because we told React to observe our `text` value, we should see our log on every keypress.

### Call an API from within `useEffect`
We can delete our `console.log()` inside of `useEffect`, and will replace it with an asynchronous function called `getStarWarsQuote`.

This function will await a `fetch` request to the API server, parse the result as JSON, and set our `text` variable to be the quote:

```js
useEffect(() => {
  async function getStarWarsQuote() {
    // Get initial text
    const response = await fetch(
      'https://swquotesapi.digitaljedi.dk/api/SWQuote/RandomStarWarsQuote'
    )
    const data = await response.json()
    const quote = data.starWarsQuote
    setText(quote)
  }

  getStarWarsQuote()
}, [])
```

Notice our empty dependency array. This is how we tell React that we only want the function inside of `useEffect` to be called once, on its initial render.

If we refresh the page, you'll see that we get our quote.

### Refactoring Gotchas

We can pull our `getStarWarsQuote` function declaration outside of `useEffect` and it will still work because it handles the asynchronous logic itself.

However, you should not pass an async function into `useEffect` directly:

```js
// THIS WILL WARN YOU
useEffect(async () => {
  // await some stuff
})
```

If you try to use `useEffect` with an async function, you'll see a warning (along with a suggestion to write it the way we did earlier)

```
The \`useEffect\` callbacks are synchronous to prevent race conditions.`
```

This helpful error message is thanks to an ESLint plugin, which comes out of the box with `create-react-app`. You can [read more about these ESLint rules on npm](https://www.npmjs.com/package/eslint-plugin-react-hooks).


