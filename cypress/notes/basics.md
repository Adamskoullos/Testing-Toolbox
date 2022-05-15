## Cypress Core

- [Test Structure](#Test-Structure)
- [get](#get)
- [contains](#contains)
- [get with contains](#get-with-contains)
- [find](#find)
- [Using Mocked Data](#Using-Mocked-Data)

---

## Test Structure

A `suite` of related tests can be scoped either within a `context` or `describe` block.
The `describe` text should state either:

- `Unit`: What component is being tested
- `Integration`: what parent component is being tested
- `E2E`: what workflow is being tested

```js
/// <reference types="cypress" />

describe("Task list page", () => {
  beforeEach(() => {
    cy.visit("/task-list");
  });
  it("");
});
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

`contains` allows us to grab a **single** element that contain a specific text.

```js
// get element by unique text
cy.contains("unique text");

// get element by text used in multiple components
cy.contains("common text"); // only grabs the first element with match

// We can use two arguments: the first being the selector and the second being the element text
cy.contains("[type='submit']", "Add task");
cy.contains("button", "Add task");
```

## get with contains

We can get multiple elements and then filter to grab a a single element

```js
cy.get("button").contains("Add task");
```

## find

`find` allows us to grab child elements of parent element. We first grab the parent with `get`/`contains` then chain `find` to grab the child:

```js
cy.get("form").find(".input-one");
```

## Using Mocked Data

If a page/component pulls in data we can mock that call and save mocked data within the `fixtures` folder.
Cypress then intercepts this call and instead returns the mocked data:

```js
it("should display a list of items", () => {
  cy.intercept("GET", "http://localhost:3000/items", {
    fixture: "items.json",
  });
  cy.get("ul").should("contain", "an items text");
});
```
