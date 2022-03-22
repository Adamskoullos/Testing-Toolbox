## Unit Testing

#### Individual Unit of Code

The below example tests a function `useFilter`:

- The two props `testArray` and `val` are dummy data passed into the function being tested, `useFilter`
- The `Arrange` is the expected result array returned from `useFilter` that is passed into the assertion method `toStrictEqual`.
- The `Act` is the returned value from `useFilter` which is passed into the assertion method `expect`
- The `Assertion` is the `expect` method from Jest comparing the Arrange value with the Act returned value

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
  // Test that component props are being correctly rendered
  // Not element specific though !
  it("Renders the title", () => {
    const wrapper = shallowMount(AppHeader, {
      propsData: {
        title: "Super Duper User Table",
      },
    });

    expect(wrapper.text()).toBe("Super Duper User Table");
  });
});
```

Element Specific example:

This example tests the actual components data not dummy data as above

```html
<h1 data-test="title">{{ title }}</h1>
```

```js
it("Renders the title", () => {
  const wrapper = shallowMount(AppHeader);
  const title = wrapper.get('[data-test="title"]');

  expect(title.text()).toBe("Super Duper User Table");
});
```
