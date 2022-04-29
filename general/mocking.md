## Mocking

When testing code we want to test the application code not 3rd party APIs, for this reason we swap out asynchronous calls with mock data.

1. To do this we first need to create a `__mocks__` folder in the same folder as the files with functions to be mocked
2. Then we create a file with the same name as the file that the function to be tested is in
3. We remove any 3rd party imports ex: axios and copy the function to be tested into the file
4. We remove the request logic and return some mock data mimicking what would be returned from the api
5. We export the function

This is an example of a function `fetchData` in `http.js`:

```js
const axios = require("axios");

const fetchData = () => {
  return axios.get("url").then((res) => res.data);
};

exports.fetchData = fetchData;
```

This is an example of the mock function `fetchData` in `http.js` within `/__mocks__`:

```js
const fetchData = () => {
  return Promise.resolve({ title: "some title" });
};

exports.fetchData = fetchData;
```

6. In the test file we need to import the mock file at the top to tell Jest to use the mock function for testing:

**Note**: The `fetchData` is a dependency of `loadTitle` below, where the title string is returned.

`util.spec.js`

```js
jest.mock("./http");

const { loadTitle } = require("./util");

test("should print an uppercase text", async () => {
  const title = await loadTitle();
  expect(title).tobe("some title");
});
```
