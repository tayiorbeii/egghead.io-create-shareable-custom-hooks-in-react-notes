# 03. Write a Custom State Hook in React
[Lesson Link](https://egghead.io/lessons/react-write-a-custom-state-hook-in-react)

Start by copying everything we've written so far in `Feedback.effect.js` into a new file called `Feedback.custom.js`. Rename the existing `FeedbackEffectComponent` references to be `FeedbackCustomComponent` instead.

Currently, we're using one piece of state right now called `text`, and we have a function called `setText` which allows us to update that value.

Up near our import statements, we're going to add a function called useText. We're calling our custom state hook useText because we're following the pattern presented by React.

Above our `FeedbackEffectComponent` declaration, we'll create a new function called `useText`. Our function's name and structure will follow the patterns used by the React team.

Our function is going to take an `initialValue`, then we'll define the function block. Inside the function body, we'll bring over our existing `useState` call and replace the empty string with our `initialValue`. From there, we'll return an array of `[text, setText]`:

```js
function useText(initialValue) {
  const [text, setText] = useState(initialValue)
  return [text, setText]
}
```

Now to use our new function, we're going to go back inside of our component and call it just like we did before with our array destructuring. This time, instead of calling useState, we're going to call useText because that's the name of our custom state hook:

```js
export function FeedbackCustomComponent() {
  const [text, setText] = useText('')
  ...
```

You can see this looks almost identical to the `useState` call we had earlier!

Inside of `App.js`, when we import and add `<FeedbackCustomComponent>`, it works as expected.

### Cleaning up our `useText` hook
Instead of destructuring `text` and `setText` and then returning a new array with those two values, we can just return the useState call:

```js
function useText(initialValue) {
  return useState(initialValue)
}
```

When we go back to the browser and verify that it's working, we see a warning from React:
```
React Hook `useEffect` has a missing dependency: `setText`.
```

We see this warning because `setText` no longer comes directly from `useState`, but instead from our custom `useText` hook. Because of that, React isn't really sure what it is, and since we're using it inside of our useEffect, it wants us to add `setText` to our dependency array just in case it changes.

```js
export function FeedbackCustomComponent {
  const [text, setText] = useText(')
  
  useEffect(() => {
    // async quote stuff
  }, [setText])
}
```

Now when we clear the console and refresh, we no longer have that warning and our `useEffect` is still working!