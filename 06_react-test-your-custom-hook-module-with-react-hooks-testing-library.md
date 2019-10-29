# 06. Test your Custom Hook Module with `react-hooks-testing-library`
[Lesson Link](https://egghead.io/lessons/react-test-your-custom-hook-module-with-react-hooks-testing-library)

We can test our custom hook module with [`react-hooks-testing-library`](https://github.com/testing-library/react-hooks-testing-library).

First we need to install `@testing-library/react-hooks` as a dev dependency inside of our custom hook module directory: 

```
yarn add -D and then @testing-library/react-hooks.
```

The next thing we'll want to do is make sure we have the correct peer dependencies. If you take a look, it says, "The minimum supported version of React and `react-test-renderer` is `16.9.0`.

To install them, we'll run:
```
yarn add -D react@16.9.0
yarn add -D react-test-renderer@16.9.0
```

Now if we run `yarn test` in a terminal, we get an error related to ESLint.

To ignore this check, we will create a `.env` file that contains this line:
```
SKIP_PREFLIGHT_CHECK=true
```

If we run the command again, we no longer get that error, but instead get a different error that no tests were found. This is because it's looking for files in the `src/` directory that end in `.spec` or `.tests` because that's the default.

Renaming the `src/test.js` file to `src/test.spec.js` we can run `yarn test` to get a test failure error due to no longer having a hook called `useMyHook`.

### Writing a Test
Inside of `src/test.spec.js`, the first thing to do is import `renderHook` from `@testing-library/react-hooks` along with importing our hook. We can also set up the `describe` where we will write our tests.

```js
import { renderHook } from '@testing-library/react-hooks'
import { useStarWarsQuote } from './'

describe('useStarWarsQuote', () => {
  //tests go in here
})
```

There are three things we want to test for:
1. Our hook returns an object with the keys `loading` and `quote`
2. `loading` is set to `true` after the initial call
3. `quote` is returned and `loading` is set to `false`.


### Test 1:
Inside of our first test, we'll create a new variable `result` by destructuring it from what is returned from calling `renderHook` and passing in a callback that calls `useStarWarsQuote()`.

```js
 test('should return an object with the keys: loading, quote', () => {
    // result.current = the value returned by our hook
    const { result } = renderHook(() => useStarWarsQuote())
```

The value returned by our hook is found at `result.current`, which means we can write assertions that it has `loading` and `quote` properties:

```js
    expect(result.current).toHaveProperty('loading')
    expect(result.current).toHaveProperty('quote')
  })
```

Running `yarn test:watch` will keep running in the background.

### Test 2
Our second test has a similar structure to the first: we can duplicate getting our `result` variable, and then assert that we expect `result.current.loading` to be `true`:

```js
test('should return an object with the keys: loading, quote', () => {
  const { result } = renderHook(() => useStarWarsQuote())
  expect(result.current.loading).toBe(true)
}
```

### Test 3
Since getting a quote is an async process, we need to update our `test` arrow function to use the `async` keyword. Then inside of the function body, we will to destructure `waitForNextUpdate` from our `renderHook` call.

We can then `await` the asynchronous `result` from `waitForNextUpdate()`.

Because the quote is random every time, we can't check it for equality against a known string. However, there are a few things we *can* test to give us the confidence that it's working properly.

We will make sure that its `typeof` is '`string`', and the `quote` is not `null` or an empty string. Finally, testing for `loading` is the same as we've used before:

```js
test('should return a quote and set loading to false', async () => {
    const { result, waitForNextUpdate } = renderHook(() => useStarWarsQuote())

    await waitForNextUpdate()
    expect(typeof result.current.quote).toBe('string')
    expect(result.current.quote).not.toBe(null)
    expect(result.current.quote).not.toBe('')
    expect(result.current.loading).toBe(false)
  })
```

Here's what the full test suite looks like all together:
```js
import { renderHook } from '@testing-library/react-hooks'
import { useStarWarsQuote } from './'

describe('useStarWarsQuote', () => {
  test('should return an object with the keys: loading, quote', () => {
    // result.current = the value returned by our hook
    const { result } = renderHook(() => useStarWarsQuote())

    expect(result.current).toHaveProperty('loading')
    expect(result.current).toHaveProperty('quote')
  })

  test('should set loading to true after initial call', () => {
    const { result } = renderHook(() => useStarWarsQuote())
    expect(result.current.loading).toBe(true)
  })

  test('should return a quote and set loading to false', async () => {
    // Add test here
    const { result, waitForNextUpdate } = renderHook(() => useStarWarsQuote())


    await waitForNextUpdate()
    expect(typeof result.current.quote).toBe('string')
    expect(result.current.quote).not.toBe(null)
    expect(result.current.quote).not.toBe('')
    expect(result.current.loading).toBe(false)
  })
})
```