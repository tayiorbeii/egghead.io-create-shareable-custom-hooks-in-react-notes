# 07. Prepare a Custom React Hook to be Published as an npm Package
[Lesson Link](https://egghead.io/lessons/react-prepare-a-custom-react-hook-to-be-published-as-an-npm-package)

To prepare a custom hook to be published as an npm package, there are a few things we need to do.

### Versioning Setup
Inside of `package.json`, change the `version` from `1.0.0` to `0.0.0`. This prepares us to use a CLI command called [`npm version`](https://docs.npmjs.com/cli/version).

We want to [use `npm version` to bump package versions](https://docs.npmjs.com/cli/version) because when run in a git repo it will create a version commit and tag.

Before we can use the npm version command, we need to make sure that we have a clean git working directory.

Commit the change made to the package.json, and then run:
```
npm version major
```

The command will print the version, and it updated it in the package.json file.

Running `git tag`, will show that there is a new commit tagged `v1.0.0``.


### Add Postpublish Script
Inside of the `scripts` section of `package.json`, we'll add an entry for `"postpublish"` that will run `git push --tags`.

```js
// Inside package.json

"scripts": {
  "prepare": "yarn run build" // runs before npm publishes the package
  ...
  "postpublish": "git push --tags" // runs after publish
}
```

### Update `peerDependencies`
The peer dependencies are the packages that are required by our package in order to make sure it works properly. Since our package is a custom React hook, we want to make sure that the user has React in their project.

Our `package.json` file specifies React version `^16.8.6`, where the `^` means "exact". 

Since we don't necessarily need this exact version, we can change this to `>=16.8.6` to accept any newer versions as well. 

### Add `publishConfig`
Finally, we'll add a new key `publishConfig` at the bottom of our `package.json` file.

Inside of this object, add an `access` key with a value of `"public"`.

This tells the npm registy that this is a public package.

```js
...
"publishConfig": {
  "access": "public"
}
```

### Update the README
The boilerplate provides a decent README, but we need to replace the Usage section with the code from `App.js`.

That's all you need to do to prepare your package to be published on npm!
