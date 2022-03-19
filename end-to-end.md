## End To End Testing

Basic example test of a user filling out a form:

**Note**: second argument can be an `async` function required when working with Puppeteer.

```js
test("Should create new element from user input", async () => {
  const browser = await puppeteer.launch({
    headless: false,
    slowMo: 80,
    args: ["--window-size=1920,1080"],
  });
  const page = await browser.newPage();
  await page.goTo("url");
  // Undertake user workflow
  await page.click("input#name");
  await page.type("input#name", "Mike");
  await page.click("input#age");
  await page.type("input#age", "25");
  await page.click("#btnAddUser");
  // Check the workflow created the element and correctly added the data
  // Grab first element get text content
  const text = await page.$eval(".user-item", (el) => el.textContent);
  // Test workflow output
  expect(text).tobe("Mike (25 years old)");
});
```
