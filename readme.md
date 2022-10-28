# Deploy truffle project to GitHub pages

Be sure that you work from the `main` or `master` folder.

```
npm install merge-dirs
```

Add this to your `package.json` file under `scripts`:


```
"contracts" : "merge-dirs ./build/contracts ./src"
```

It should look something like this:

```
"scripts": {
    "dev": "lite-server",
    "contracts" : "merge-dirs ./build/contracts ./src"
  },
```

Create a `.github/workflows` folder in the root of your project.

Create main.yml file in that folder.

Add this to that folder.

```
name: Build and Deploy
on: [push]
permissions:
  contents: write
jobs:
  build-and-deploy:
    concurrency: ci-${{ github.ref }} # Recommended if you intend to make multiple deployments in quick succession.
    runs-on: ubuntu-latest
    steps:
      - name: Checkout 🛎️
        uses: actions/checkout@v3

      - name: Install and Build 🔧 # This example project is built using npm and outputs the result to the 'build' folder. Replace with the commands required to build your project, or remove this step entirely if your site is pre-built.
        run: |
          npm ci
          npm run contracts

      - name: Deploy 🚀
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          folder: src # The folder the action should deploy.
```

Pushing to GitHub pages will deploy the `src` folder to GitHub pages.

The `npm run contracts` copy the contracts into the root folder of a given project.

## Moving to the main branch

You might need to move from the `gh-pages` branch to the `main` branch.


From the main branch
```
git branch main
git push origin main
```

You might need to remove the `gh-pages` branch locally.

```
git branch -d gh-pages
git push origin gh-pages
```