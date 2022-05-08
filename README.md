## Testing-Toolbox

### React Testing Library

- [Tools](#Tools)
- [Test and Describe Blocks](#Test-and-Describe-Blocks)
- [Query Methods](#Query-Methods)
- [Querying Attributes Best Practice](#Querying-Attributes-Best-Practice)
- [Query Patterns](#Query-Patterns)
- [Assertions](#Assertions)
- [Events and Interacting with Elements](#Events-and-Interacting-with-Elements) (including a mocked function)
- [Integration Testing](#Integration-Testing)

### Cypress Testing

- [Setup]()
- [Cypress Basics]()
- [Cypress + Testing-Library React]()

---

## Tools

- `Unit Tests` > React testing library > Single component
- `Integration Tests` > React testing library > Multiple components
- `Unit | Integration | E2E Tests` > Cypress

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
import { render, screen, fireEvent } from "@testing-library/react";
import AddInput from "../AddInput";

// Mocked function that does nothing
const mockedSetTodo = jest.fn();

describe("AddInput", () => {
  beforeAll(() => {
    // code to run once before all test
  });

  beforeEach(() => {
    // code to run before each test
  });

  afterAll(() => {
    // code to run once after all test
  });

  afterEach(() => {
    // code to run after each test
  });

  it("should render input element", async () => {
    render(<AddInput todos={[]} setTodos={mockedSetTodo} />);
    const inputEl = screen.getByPlaceholderText("Add a new task...");
    expect(inputEl).toBeInTheDocument();
  });

  it("should be able to type into the input", async () => {
    render(<AddInput todos={[]} setTodos={mockedSetTodo} />);
    const inputEl = screen.getByPlaceholderText("Add a new task...");

    // Interact with elements (update input element value)
    fireEvent.change(inputEl, { target: { value: "Buy bananas" } });
    expect(inputEl.value).toBe("Buy bananas");
  });

  it("should be able to click the add button and reset the input value", async () => {
    // Render component
    render(<AddInput todos={[]} setTodos={mockedSetTodo} />);

    // Grab elements
    const inputEl = screen.getByPlaceholderText("Add a new task...");
    const addButtonEl = screen.getByRole("button", { name: "Add" });

    // Interact with elements (update input element value, click button, reset to empty string)
    fireEvent.change(inputEl, { target: { value: "Buy bananas" } });
    fireEvent.click(addButtonEl);
    expect(inputEl.value).toBe("");
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

[Back to top](#Testing-Toolbox)

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

[Back to top](#Testing-Toolbox)

---

## Events and Interacting with Elements

The below examples are for an input component that includes an input and add button.

```js
import { render, screen, fireEvent } from "@testing-library/react";
import AddInput from "../AddInput";

// Mocked function that does nothing
const mockedSetTodo = jest.fn();

describe("AddInput", () => {
  it("should render input element", async () => {
    render(<AddInput todos={[]} setTodos={mockedSetTodo} />);
    const inputEl = screen.getByPlaceholderText("Add a new task...");
    expect(inputEl).toBeInTheDocument();
  });

  it("should be able to type into the input", async () => {
    render(<AddInput todos={[]} setTodos={mockedSetTodo} />);
    const inputEl = screen.getByPlaceholderText("Add a new task...");

    // Interact with elements (update input element value)
    fireEvent.change(inputEl, { target: { value: "Buy bananas" } });
    expect(inputEl.value).toBe("Buy bananas");
  });

  it("should be able to click the add button and reset the input value", async () => {
    // Render component
    render(<AddInput todos={[]} setTodos={mockedSetTodo} />);

    // Grab elements
    const inputEl = screen.getByPlaceholderText("Add a new task...");
    const addButtonEl = screen.getByRole("button", { name: "Add" });

    // Interact with elements (update input element value, click button, reset to empty string)
    fireEvent.change(inputEl, { target: { value: "Buy bananas" } });
    fireEvent.click(addButtonEl);
    expect(inputEl.value).toBe("");
  });
});
```

[Back to top](#Testing-Toolbox)

---

## Integration Testing

The below example tests the integration of two components:

1. AddTask
2. TaskList

`Todo` is the parent component.

Because `AddTodo` has `Link` we will wrap the parent in the `BrowserRouter` tags and use the MockTodo.
Rendering the parent component `Todo` also renders both `AddTodo` and `TaskList` making their elements available.

The first example below shows a single task being created in `AddTodo` and then added to/populated in `TodoList`:

```js
import { render, screen, fireEvent } from "@testing-library/react";
import Todo from "../Todo";
import AddTodo from "../AddTodo";
import TaskList from "../TaskList";
import { BrowserRouter } from "react-router-dom";

const MockTodo = () => {
  return (
    <BrowserRouter>
      <Todo />
    </BrowserRouter>
  );
};
describe("Todo", () => {
  it("should render task element", async () => {
    // Render parent component
    render(<MockTodo />);
    // Elements in AddTodo
    const inputEl = screen.getByPlaceholderText("Add a new task...");
    const addButtonEl = screen.getByRole("button", { name: "Add" });
    // Interact with elements (Add todo)
    fireEvent.change(inputEl, { target: { value: "Buy bananas" } });
    fireEvent.click(addButtonEl, { name: "Add new task..." });
    // Grab elements in TodoList
    const todoListItemEl = screen.getByText("Buy bananas");
    // Assert that the new task is in the TodoList component
    expect(todoListItem).toBeInTheDocument();
  });
});
```

Following on from the above we can use a function to create multiple and add tasks to the TodoList component so we can test that an array of tasks is being correctly created and rendered:

```js
const addTask = (tasks) => {
  // Elements in AddTodo
  const inputEl = screen.getByPlaceholderText("Add a new task...");
  const addButtonEl = screen.getByRole("button", { name: "Add" });
  // Add task for each item in tasks
  tasks.forEach((task, index) => {
    // Interact with elements (Add todo)
    fireEvent.change(inputEl, { target: { value: task } });
    fireEvent.click(addButtonEl, { name: "Add new task..." });
  });
};

describe("Todo", () => {
  it("should render multiple tasks elements", async () => {
    // Render parent component
    render(<MockTodo />);
    // Add multiple tasks
    addTask(["buy bananas", "wash up", "iron jeans", "eat cake"]);
    // Grab elements in TodoList
    const taskElements = screen.getAllByTestId("task");
    // Assert that the new task is in the TodoList component
    expect(taskElements.length).toBe(3);
  });
});
```

We can also check for elements to have or not have class names. The below example asserts that new tasks do have not the completed class:

```js
describe("Todo", () => {
  it("should render tasks as incomplete", async () => {
    // Render parent component
    render(<MockTodo />);
    addTask(["buy bananas"]);
    const taskElement = screen.getByText("buy bananas");
    expect(taskElement).not.toHaveClass("complete");
  });
});
```

The next example tests that if a tasklist is clicked and set to complete that it is assigned the `complete` class:

```js
describe("Todo", () => {
  it("should add class complete when clicked", async () => {
    // Render parent component
    render(<MockTodo />);
    addTask(["buy bananas"]);
    const taskElement = screen.getByText("buy bananas");
    fireEvent.click(taskElement);
    expect(taskElement).toHaveClass("complete");
  });
});
```

[Back to top](#Testing-Toolbox)

---
