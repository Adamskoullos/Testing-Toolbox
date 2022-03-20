# Testing-Toolbox

- [Unit Testing](https://github.com/Adamskoullos/Testing-Toolbox/blob/main/unit-testing.md)
- [Integration Testing]() > Todo
- [End-To-End Testing](https://github.com/Adamskoullos/Testing-Toolbox/blob/main/end-to-end.md)
- [Mocking](https://github.com/Adamskoullos/Testing-Toolbox/blob/main/mocking.md)

---

### Unit Testing

Test isolated `functions` and `components`

To create effective unit tests logic needs to be broken down into functions/units that undertake a single meaningful task and do not have dependencies.

- Each unit test can be run independently from any other unit test.

- External dependencies are managed with `Doubles` > Mocks, Fakes, Stubs:
  - `Mocks`> Is the function being called properly or not
    - Is the mock function called
    - How many times is it getting called
    - What params are passed in when it is called
  - `Stubs` > Generate pre-defined outputs
  - `Fakes` > Used for testing code a unit of code handles data from a test db

Universal structure of a unit test `AAA`:

- `Arrange`: Dummy data is saved to a property that is used as the target match for the `Act` returned value
- `Act`: Act is the returned value from the tested unit of code
- `Assert`: The `Assertion` is the function that compares the `Arrange` with the `Act` and provides a pass or fail for the unit test.

### Integration Testing

Test `functions` with dependencies

Integration tests chunk a group of units that would be equivalent to a large function undertaking several tasks within a workflow. The purpose is to ensure each unit as a dependency integrates correctly with each other unit.

### End-To-End Testing

Test `user workflows`, to add this into the automated testing process within the pipeline we need to use a `headless browser` such as `Jest and Puppeteer` or `Cypress`

An `End-To-End` test includes the code from all the `units` within the code tested within potentially multiple `integration` tests for a complete user workflow. For example when a user completes and posts a new form to the database.

---
