# Testing-Toolbox

- [Unit Testing]()
- [Integration Testing]()
- [End-To-End Testing](https://github.com/Adamskoullos/Testing-Toolbox/blob/main/end-to-end.md)

---

### Unit Testing

Test isolated `functions` and `components`

To create effective unit tests logic needs to be broken down into functions that undertake a single task and do not have dependencies.

### Integration Testing

Test `functions` with dependencies

Integration tests chunk a group of units that would be equivalent to a large function undertaking several tasks within a workflow. The purpose is to ensure each unit as a dependency integrates correctly with each other unit.

### End-To-End Testing

Test `user workflows`, to add this into the automated testing process within the pipeline we need to use a `headless browser` such as `Puppeteer`

An `End-To-End` test includes the code from all the `units` within the code tested within potentially multiple `integration` tests for a complete user workflow. For example when a user completes and posts a new form to the database.

---
