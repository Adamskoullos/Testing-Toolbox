## Test Driven Development

Basic process for `components`:

1. Create boiler plate component
2. Create boiler plate `spec` file
3. Create the first test
4. Test should fail and the `red` milestone has been met
5. Build out the component just enough so the components tests pass and the `green` milestone has been met
6. Refactor the component to meet specification whilst maintaining a passing test.

Step 6 can involve multiple iterations with tests being refined/added ahead of each refactor so the finished article is robustly tested. The act of creating a test for each refactor promotes a well thought out development process.

---

### Step 1 and 2

**Boilerplate spec file**:

```js
import { shallowMount } from "@vue/test-utils";
import Rating from "@/components/Rating";

let wrapper = null;

beforeEach(() => {
  wrapper = shallowMount(Rating);
});
afterEach(() => {
  wrapper.destroy();
});

describe("Rating", () => {});
```

```html
<template>
  <div></div>
</template>
```

### Step 3

**Create tests**:
Within the `describe` block create either an `it` or `test` method for each individual test.

```js
describe("Rating", () => {
  it("It renders the rating stars", () => {
    const stars = wrapper.findAll(".star");
    expect(star.length).toBe(5);
  });
});
```

> **Step 4**: code is initially tested with an expected fail

### Step 5

Build out the component just enough to pass the first test.

```html
<template>
  <div>
    <ul>
      <li class="star"></li>
      <li class="star"></li>
      <li class="star"></li>
      <li class="star"></li>
      <li class="star"></li>
      <li class="star"></li>
    </ul>
  </div>
</template>
```

### Step 6

Refine and add tests as required as the component is refactored

**Iteration 1**: > Pass props in so the number of stars can be dynamic:

**First update the test**: > Add props

```js
beforeEach(() => {
  wrapper = shallowMount(Rating, {
    propsData: {
      maxStars: 5,
    },
  });
});
afterEach(() => {
  wrapper.destroy();
});

describe("Rating", () => {
  it("It renders the rating stars", () => {
    const stars = wrapper.findAll(".star");
    expect(star.length).toBe(5);
  });
});
```

Develop the component to use props:

```html
<template>
  <div>
    <ul>
      <li v-for="star in maxStars" :key="star" class="star"></li>
    </ul>
  </div>
</template>
```

```js
<script>
export default {
    props: {
        maxStars: {
            type: Number,
        }
    }
}
</script>
```

**Iteration 2**: > Test the component is rendering star rating:

**First update the test**: > Add prop for star rating (active) and add test

```js
beforeEach(() => {
  wrapper = shallowMount(Rating, {
    propsData: {
      maxStars: 5,
      active: 2,
    },
  });
});
afterEach(() => {
  wrapper.destroy();
});

describe("Rating", () => {
  it("It renders the rating stars", () => {
    const stars = wrapper.findAll(".star");
    expect(star.length).toBe(5);
  });
  it("It renders active stars", () => {
    const active = wrapper.findAll(".active");
    expect(active.length).toBe(2);
  });
});
```

Test created and ran which now fails

**Refactor the component to pass**: > Add prop for star rating (active) and add a conditional class in v-for

```html
<template>
  <div>
    <ul>
      <li
        v-for="star in maxStars"
        :key="star"
        class="star"
        :class="{active: star <= active}"
      ></li>
    </ul>
  </div>
</template>
```

```js
<script>
export default {
    props: {
        maxStars: {
            type: Number,
        },
        active: {
            type: Number,
        }
    }
}
</script>
```
