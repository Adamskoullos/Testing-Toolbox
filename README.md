## Testing-Toolbox

- [Tools](#Tools)
- [Test and Describe Blocks](#Test-and-Describe-Blocks)
- [Query Methods](#Query-Methods)
- [Querying Attributes Best Practice](#Querying-Attributes-Best-Practice)
- [Query Patterns](#Query-Patterns)
- [Assertions](#Assertions)
- [Events and Interacting with Elements](#Events-and-Interacting-with-Elements)

---

## Tools

- `Unit Tests` > React testing library
- `Integration Tests` > React testing library
- `End-To-End Tests` > Cypress

---

## Test and Describe Blocks

Each test block does the following:

1. `Render` the component we want to test
2. `Find` and `grab` the element(s) within the component that we want to test
3. `Interact` with those elements
4. `Assert` that the results are as expected

A group of tests for a specific component can be grouped within a `describe block`. We can further organize tests by using nested `describe blocks`:

- parent describe block
  - child one describe block
    - test block one
    - test block two
  - child two describe block
    - test block one
    - test block two

```js
import { render, screen } from "@testing-library/react";
import SomeComponent from "./someComponent";

describe('SomeComponent', () => {
    // ByText
    it("should render the passed in title text", () => {
        // Render the component into the virtual dom
        render(<SomeComponent title="My Header" />);
        // Grab elements to test
        const headingElement = screen.getByText(/My Header/i);
        // Interact with elements (not used in this test)

        // Assert if results are as expected
        expect(headingElement).toBeInTheDocument();
    });
    // ByRole
    it("should render the heading", () => {
        // Render the component into the virtual dom
        render(<SomeComponent />);
        // Grab elements to test
        const headingElement = screen.getByRole('heading'));
        // Interact with elements (not used in this test)

        // Assert if results are as expected
        expect(headingElement).toBeInTheDocument();
    });
});
```

---

## Query Methods

The imported `screen` object has many methods for querying the virtual dom and grabbing elements.
There are three main types:

- `getBy` / `getAllBy`
- `findBy` / `findAllBy`
- `queryBy` / `queryAllBy`

|              | getBy  | findBy | queryBy | getAllBy | findAllBy | queyAllBy     |
| ------------ | ------ | ------ | ------- | -------- | --------- | ------------- |
| **No Match** | error  | error  | `null`  | error    | error     | `empty array` |
| **1 Match**  | return | return | return  | array    | array     | array         |
| **1+ Match** | error  | error  | error   | array    | array     | array         |
| **Await**    | no     | `yes`  | no      | no       | `yes`     | no            |

---

## Querying Attributes Best Practice

The approach is to query elements as close to how the browser would from an accessibility first perspective and work down from there as needed:

| `Accessible By Everyone` |
| ------------------------ |
| getByRole                |
| getByLabelText           |
| getByPlaceholderText     |
| getByText                |
| **`Semantic Queries`**   |
| getByAltText             |
| getByTitle               |
| **`Test ID`**            |
| getByTestId              |

---

## Query Patterns

`getByText`

```js
it("should render the passed in title text", () => {
  // Render the component into the virtual dom
  render(<SomeComponent title="My Header" />);
  // Grab elements to test
  const headingElement = screen.getByText(/My Header/i);
  // Interact with elements (not used in this test)

  // Assert if results are as expected
  expect(headingElement).toBeInTheDocument();
});
```

`getByRole`

```js
it("should render the heading", () => {
  // Render the component into the virtual dom
  render(<SomeComponent />);
  // Grab elements to test
  const headingElement = screen.getByRole('heading'));
  // Interact with elements (not used in this test)

  // Assert if results are as expected
  expect(headingElement).toBeInTheDocument();
});
```

`getByRole & Text`

The second argument is an object and `name` is used to match the text:

```js
it("should render the heading", () => {
  // Render the component into the virtual dom
  render(<SomeComponent title="My Header" />);
  // Grab elements to test
  const headingElement = screen.getByRole('heading', { name: 'My Header' }));
  // Interact with elements (not used in this test)

  // Assert if results are as expected
  expect(headingElement).toBeInTheDocument();
});
```

`findByText`

We can use `findBy` when we need to use `async` code:

```js
it("should render the passed in title text", async () => {
  // Render the component into the virtual dom
  render(<SomeComponent title="My Header" />);
  // Grab elements to test
  const headingElement = await screen.findByText(/My Header/i);
  // Interact with elements (not used in this test)

  // Assert if results are as expected
  expect(headingElement).toBeInTheDocument();
});
```

`queryByText`

We can use `queryBy` when we need to assert that an element is not in the virtual dom:

```js
it("should not render the old header", () => {
  // Render the component into the virtual dom
  render(<SomeComponent title="New Header" />);
  // Grab elements to test
  const oldHeadingElement = screen.queryByText(/Old Header/i);
  // Interact with elements (not used in this test)

  // Assert if results are as expected
  expect(oldHeadingElement).not.toBeInTheDocument();
});
```

`getAllByRole`

```js
it("should render all headings", () => {
  // Render the component into the virtual dom
  render(<SomeComponent title="New Header" subTitle="Sub Title" />);
  // Grab elements to test
  const headingElements = screen.getAllByRole("heading");
  // Interact with elements (not used in this test)

  // Assert if results are as expected
  expect(headingElements.length).toBe(2);
});
```

---

## Assertions

When testing components with react router `Link` tags we need to mock a wrapper component so `Link` is within `BrowserRouter `:

`toBeInTheDocument`

```js
import SomeComponent from "./SomeComponent";
import { BrowserRouter } from "react-router-dom";

const MockSomeComponent = ({ incompleteTasks }) => {
  return (
    <BrowserRouter>
      <SomeComponent incompleteTasks={incompleteTasks} />
    </BrowserRouter>
  );
};

it("should render the correct amount of incomplete tasks", () => {
  // Render the component into the virtual dom
  render(<MockSomeComponent incompleteTasks={5} />);
  // Grab elements to test
  const paragraphElement = screen.getByText(/5 tasks left/i);
  // Interact with elements (not used in this test)

  // Assert if results are as expected
  expect(paragraphElement).toBeInTheDocument();
});
```

`toBeTruthy & toBeFalsy`

Following on the above, assuming there is some logic that sets the text to say `task` if the number of incomplete tasks is 1.

This time we are using `toBeTruthy` which asserts as a boolean that the element is in the dom:

```js
it("should render `task` if incomplete tasks is 1", () => {
  // Render the component into the virtual dom
  render(<MockSomeComponent incompleteTasks={1} />);
  // Grab elements to test
  const paragraphElement = screen.getByText(/1 task left/i);
  // Interact with elements (not used in this test)

  // Assert if results are as expected
  expect(paragraphElement).toBeTruthy();
});
```

`toBeVisible`

```js
it("should be visible to the user", () => {
  // Render the component into the virtual dom
  render(<MockSomeComponent incompleteTasks={1} />);
  // Grab elements to test
  const paragraphElement = screen.getByText(/1 task left/i);
  // Interact with elements (not used in this test)

  // Assert if results are as expected
  expect(paragraphElement).toBeVisible();
});
```

`toBe`

We can use the `.textContent` in conjunction with `toBe` to match text content with expected value:

```js
it("should be visible to the user", () => {
  // code...
  // Assert if results are as expected
  expect(paragraphElement.textContent).toBe("some text");
});
```

---

## Events and Interacting with Elements
