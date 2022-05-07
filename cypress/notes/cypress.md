## Cypress

- [Setup](#Setup)
- [Get Elements]

---

## Setup

Also installing the testing-library allows us to use the methods such as findBy and queryBy:

```
npm install -D cypress @testing-library/cypress eslint-plugin-cypress
```

Add `script` to `package.json` to open the testing UI:

```json
{
  "cypress": "cypress open"
}
```

Then create `jsconfig.json` file and add either or the following for intellisense:

For JS:

```json
{
  "include": ["node_modules/cypress", "./cypress/**/*.js"]
}
```

For TypeScript:

```json
{
  "compilerOptions": {
    "types": ["cypress", "@testing-library/cypress"]
  }
}
```

Add the cypress testing-library commands so we can use `cy.`.

`cypress/support/commands.js`:

```
import '@testing-library/cypress/add-commands'
```

Add a `.eslintrc.json` file to cypress directory with configuration:

`cypress/.eslintrc.json`:

```json
{
  "extends": ["plugin:cypress/recommended"]
}
```

We can configure the `cypress.json` file:

> We can actually just remove the default example `/fixtures` files and `/examples` folder.

```json
{
  "baseUrl": "http://localhost:3000",
  "ignoreTestFiles": "**/examples/*",
  "viewportWidth": "1920",
  "viewportHeight": "1080"
}
```

Then add to `.gitignore`:

```
# testing
/coverage
/cypress/videos
/cypress/screenshots
```

Open cypress UI:

```
npm run cypress
```
