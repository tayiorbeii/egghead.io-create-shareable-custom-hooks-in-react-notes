#04. Write a Custom Effect Hook in React
[Lesson Link](https://egghead.io/lessons/react-write-a-custom-react-effect-hook)

Inside of our `Feedback.custom.js` file, we'll declare a new function called `useStarWarsQuote`, and paste in our existing `useEffect` code.

```js
function useStarWarsQuote() {
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
}
```

The first refactoring we need to do is to keep track of our quote in state with the `useState` hook. We'll do this by destructuring `quote` and `setQuote` from the array that is returned from calling `useState()

We'll declare a const and we're going to call it quote and set quote Eagles useState, and we're going to pass in an empty string to initialize it. We'll also replace `setText()` with `setQuote()` and pass it our quote. Finally, we'll return an object containing our quote:

```js
function useStarWarsQuote() {
  const [quote, setQuote] = useState('')
  useEffect(() => {
    async function getStarWarsQuote() {
      const response = await fetch(
        'https://swquotesapi.digitaljedi.dk/api/SWQuote/RandomStarWarsQuote'
      )
      const data = await response.json()
      const quote = data.starWarsQuote
      
      // before:
      // setText(quote)
      // after:
      setQuote(quote)
    }

    getStarWarsQuote()
  }, [])

  return {quote}
}
```

Now we'll track a `loading` state.

Below the `quote` and `setQuote` declarations, we'll declare `loading` and `setLoading` with a default value of `false`. We'll call `setLoading(true)` as the first thing that happens in our `getStarWarsQuote()` function, and then set `loading` to `false` after we set our `quote`. From there, we'll add `loading` to the object that we return.
```js
function useStarWarsQuote() {
  const [quote, setQuote] = useState('')
  const [loading, setLoading] = useState(false)
  useEffect(() => {
    async function getStarWarsQuote() {
      setLoading(true)
      // fetch the quote
      setQuote(quote)
      setLoading(false)
    }

    getStarWarsQuote()
  }, [])

  return {quote, loading}
}
```

### Using our custom `useStarWarsQuote` Hook
Inside of `<FeedbackCustomComponent />`, we'll destructure `quote` and `loading` from calling `useStarWarsQuote()`:
```js
const {quote, loading} = useStarWarsQuote()
```

Above our `handleSubmit` function, we'll add a `useEffect` call. Inside of the anonymous arrow function inside of `useEffect`, we'll do a check that will call `setText()` if there's a `quote`. Remember to add `quote` to the dependency array so the effect is called every time `quote` changes!

```js
export function FeedbackCustomComponent() {
  const [text, setText] = useText('')
  const {quote, loading} = useStarWarsQuote()

  useEffect(() => {
    if (quote) {
      setText(quote)
    }
  }, [quote])
}
```

### Add "Loading" Message

Above the component's `return`, we can add a conditional check that will return a "Loading..." message appropriately:
```js
if (loading) return <p>Loading...</p>

return (
  <Form ... />
```