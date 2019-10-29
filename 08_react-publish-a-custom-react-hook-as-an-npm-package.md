08. Publish a Custom React Hook as an npm Package
[Lesson Link](https://egghead.io/lessons/react-publish-a-custom-react-hook-as-an-npm-package)

If you don't have an npm account, you can go to [npmjs.com](https://npmjs.com) and click on "join" to create a free account.

In a terminal, log in to npm with your credentials by running `npm login`


If all is successful, you'll see "Logged in as your username on registry.npmjs.org".

Since we already have the `prepare` script in place, we don't need to run `yarn build` because npm will automatically run it when we run `npm publish`.

Run `npm publish`

Note that it ran the postpublish script `git push --tags`.

Now if we head over to the npm website, click on your avatar, click on packages, you should see your package along with the README you wrote and a link to the git repo.

That's all you need to do to publish your custom React hook as an npm package!
