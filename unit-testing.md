## Unit Testing

#### Individual Unit of Code

The below example tests a function `useFilter`:

- The `Arrange` includes two dummy props `testArray` and `val`.
- The `Act` is to test `useFilter` passing in the dummy props.
- The `Assertion` is the `expect` method from Jest.

```js
import useFilter from "../../src/utils/useFilter";

const testArray = [
  ["0", "match", "2", "3", "4", "5", "6"],
  ["0", "1", "2", "3", "4", "5", "6"],
  ["0", "1", "2", "3", "4", "5", "match"],
];
const val = "match";

test("Return all items with a matching string", () => {
  expect(useFilter(val, testArray)).toStrictEqual([
    ["0", "match", "2", "3", "4", "5", "6"],
    ["0", "1", "2", "3", "4", "5", "match"],
  ]);
});
```

#### Vue Component

```js
import {
  enableAutoDestroy,
  resetAutoDestroyState,
  shallowMount,
} from "@vue/test-utils";

import AppHeader from "../../src/components/AppHeader.vue";

// calls wrapper.destroy() after each test
enableAutoDestroy(afterEach);
// resets auto-destroy after suite completes
afterAll(resetAutoDestroyState);

describe("AppHeader.vue", () => {
  // Tests that the component has an image
  it("Renders the logo", () => {
    const wrapper = shallowMount(AppHeader);

    expect(wrapper.find("img").exists()).toBe(true);
  });
  // Tests the component has a title
  // and the passed in title is successfully being used
  it("Renders the title", () => {
    const title = "Super Duper User Table";
    const wrapper = shallowMount(AppHeader, {
      propsData: { title },
    });

    expect(wrapper.find("h1").text()).toMatch(title);
  });
});
```
