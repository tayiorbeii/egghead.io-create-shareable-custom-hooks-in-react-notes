05. Extract a Custom Hook into its Own Module with `create-react-hook`
[Lesson Link](https://egghead.io/lessons/react-extract-a-custom-hook-into-it-s-own-module-with-create-react-hook)

Inside of a new terminal window, check your npm version with `npm --version`. 

We're going to be using `npx`, which comes with npm versions 5.2 and above.

Now run `npx create-react-hook`, and prepare to answer some questions to get the boilerplate set up for the custom hook package.

For the package name, use `@yourNpmRegistryUsername/customHookName`.

The reason that we're prefixing with our npm registry username is to ensure the package is scoped to our account. This means we don't have to worry about any name conflicts if the `customHookName` package was already taken.

Add a description, and set the GitHub repo path (likely matching the npm package name with your username prefix).

In the Star Wars Quote example, we use MIT for license, yarn as the package manager, and "default" for templates.

Hit enter, and it will install our dependencies and set up the boilerplate.

An `example/` folder will be created with a `create-react-app` App for our custom hook, and `yarn link` will be set up so that we can test our custom hook.

Once the CLI finishes, open the resulting directory in an editor, and take a look at what `create-react-hook` set up for us. There are several different files and folders folders: There's a README, `package.json`, Travis, LCI, etc. We can ignore most of them.

We will add our hook inside of `index.js` in the `src/` directory.
The `example/` directory is gives us a place for us to test our hook locally.
The `dist/` directory is where rollup is going to put our bundled output.

Inside of the `package.json` file, notice that the npm module name we provided is listed as a `link` under the dependencies section. This allows us to use our hook directly inside of `App.js` (which has an example called `useMyHook` as default).

### Adding the Custom `useStarWarsQuote` Hook
Copy the code for our `useStarWarsQuote` Hook, and replace the `useMyHook` boilerplate code from `src/index.js`.
Add `export` to the function declaration, and make sure to import `useState` and `useEffect` from React at the top of the file.

```js
// custom-hook-dir/src/index.js
import { useState, useEffect } from 'react'

export function useStarWarsQuote() {
  ...
```

### Testing our Custom Hook
Inside of  `example/App.js`, and change the import from `useMyHook` to `useStarWarsQuote`. We know that `useStarWarsQuote` returns an object with `quote` and `loading`, and so we can set up the conditional render like we did before.

Back in the terminal, run `yarn start` to run rollup in "watch" mode. This will bundle the custom hook code.
In another terminal window, navigate to the `example` directory and run `yarn start` to launch the example `create-react-app` project.

Now if we go into the browser, to http://localhost:3000, we'll see that our custom hook is working!
