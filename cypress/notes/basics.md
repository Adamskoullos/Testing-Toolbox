## Cypress Core

- [Test Structure]()
- [get]()
- [contains]()
- [find]()

---

## Test Structure

A `suite` of related tests can be scoped either within a `context` or `describe` block.
The `describe` text should state either:

- `Unit`: What component is being tested
- `Integration`: what parent component is being tested
- `E2E`: what workflow is being tested

```js
/// <reference types="cypress" />

describe("", () => {});
```

---

## get

1. `get` grabs all the elements that fit the criteria
2. `get` can grab a single element or multiple

There are multiple ways we can grab elements:

```js
// get by data test id
cy.get("[data-cy='btn-id-1']");

// get by tag name
cy.get("button");

// get by class
cy.get(".btn-class");

// get by id
cy.get("#btn-id");

// get by tag name and class
cy.get("button.btn-submit");

// get element with specific classes using the attribute selector
cy.get("[class='class-one class-two']");

// get element with multiple criteria: tag, class, attribute
cy.get("button.btn-submit[type='submit']");
```

---

## contains

`contains` allows us to grab elements that contain a specific text.
