---
title: "ElementHandle"
excerpt: "xk6-browser: ElementHandle Class"
---

> ️ **️⚠️ Compatibility**
> 
> The [xk6-browser API](/javascript-api/k6-x-browser/) aims for rough compatibility with the [Playwright API for NodeJS](https://playwright.dev/docs/api/class-playwright). 
> 
> Because k6 does not run in NodeJS, the xk6-browser API will slightly differ from its Playwright counterpart.
> 
> Note that k6 APIs are synchronous.


## Supported APIs

Support for the [`ElementHandle` Playwright API](https://playwright.dev/docs/api/class-elementhandle) except for the following APIs:

### Missing Playwright APIs

<Glossary>

- [$eval()](https://playwright.dev/docs/api/class-elementhandle#element-handle-eval-on-selector)
- [$$eval()](https://playwright.dev/docs/api/class-elementhandle#element-handle-eval-on-selector-all)
- [screenshot()](https://playwright.dev/docs/api/class-elementhandle#element-handle-screenshot)
- [setInputFiles()](https://playwright.dev/docs/api/class-elementhandle#element-handle-set-input-files)

</Glossary>

🚧 `xk6-browser` is in Beta - we are working to cover more Playwright APIs.

## Examples

<CodeGroup labels={["Fill out a form"]} >

```javascript
import launcher from 'k6/x/browser';

export default function () {
  const browser = launcher.launch('chromium', {
    headless: false,
    slowMo: '500ms', // slow down by 500ms
  });
  const context = browser.newContext();
  const page = context.newPage();

  // Goto front page, find login link and click it
  page.goto('https://test.k6.io/', { waitUntil: 'networkidle' });
  const elem = page.$('a[href="/my_messages.php"]');
  elem.click();

  // Enter login credentials and login
  page.$('input[name="login"]').type('admin');
  page.$('input[name="password"]').type('123');
  page.$('input[type="submit"]').click();

  // Wait for next page to load
  page.waitForLoadState('networkdidle');

  page.close();
  browser.close();
}
```

</CodeGroup>

<CodeGroup labels={["Check element state"]} >

```javascript
import launcher from 'k6/x/browser';
import { check } from 'k6';

export default function () {
  const browser = launcher.launch('chromium', {
    headless: false,
  });
  const context = browser.newContext();
  const page = context.newPage();

  // Inject page content
  page.setContent(`
        <div class="visible">Hello world</div>
        <div style="display:none" class="hidden"></div>
        <div class="editable" editable>Edit me</div>
        <input type="checkbox" enabled class="enabled">
        <input type="checkbox" disabled class="disabled">
        <input type="checkbox" checked class="checked">
        <input type="checkbox" class="unchecked">
    `);

  // Check state
  check(page, {
    visible: (p) => p.$('.visible').isVisible(),
    hidden: (p) => p.$('.hidden').isHidden(),
    editable: (p) => p.$('.editable').isEditable(),
    enabled: (p) => p.$('.enabled').isEnabled(),
    disabled: (p) => p.$('.disabled').isDisabled(),
    checked: (p) => p.$('.checked').isChecked(),
    unchecked: (p) => p.$('.unchecked').isChecked() === false,
  });

  page.close();
  browser.close();
}
```

</CodeGroup>


### Browser APIs

<Glossary>

-  [Browser](/javascript-api/k6-x-browser/browser/)
-  [BrowserContext](/javascript-api/k6-x-browser/browsercontext/)
-  [BrowserType](/javascript-api/k6-x-browser/browsertype/)
-  [ElementHandle](/javascript-api/k6-x-browser/elementhandle/)
-  [Frame](/javascript-api/k6-x-browser/frame/)
-  [JSHandle](/javascript-api/k6-x-browser/jshandle)
-  [Keyboard](/javascript-api/k6-x-browser/keyboard)
-  [Mouse](/javascript-api/k6-x-browser/mouse/)
-  [Page](/javascript-api/k6-x-browser/page/)
-  [Request](/javascript-api/k6-x-browser/request/)
-  [Response](/javascript-api/k6-x-browser/response/)
-  [Browser](/javascript-api/k6-x-browser/browser/)
-  [Touchscreen](/javascript-api/k6-x-browser/touchscreen/)

</Glossary>