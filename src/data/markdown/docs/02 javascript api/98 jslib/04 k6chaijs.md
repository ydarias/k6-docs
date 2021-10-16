---
title: "k6chaijs"
excerpt: "Assertion library for k6"
---

The `k6chaijs` is a wrapper around [Chai Assertion Library](https://www.chaijs.com/) that makes it work with k6. 

Chaijs is a more powerful alternative to the k6-native `check()` and `group()` functionality. This library allows developers to use more familiar style of specifying assertions.

This library is especially useful for:
 - Functional testing, where many asserts are needed
 - Stress testing, where the System Under Test is failing and the test code needs to stay robust.
 - Load testing, when the test should be aborted as soon as the first failure occurs.

> ⭐️ Source code available on [GitHub](https://github.com/grafana/k6-jslib-k6chaijs). 
> Please request features and report bugs through [GitHub issues](https://github.com/grafana/k6-jslib-k6chaijs/issues).

<Blockquote mod='info'>

## Installation
There's nothing to install. This library is hosted on [jslib](https://jslib.k6.io/) and can be imported in the k6 script directly.

</Blockquote>


<CodeGroup labels={[]}>

```javascript
import { describe, expect } from 'https://jslib.k6.io/k6chaijs/4.3.4.0/index.js';
```

</CodeGroup>

Alternatively, you can use a copy of this file stored locally.

## Simple example

Let's get started by writing a test for a hypothetical HTTP API that should return a JSON array of objects. 

First, create a `mytest.js` k6 script file.


<CodeGroup labels={[]}>

```javascript
import { describe, expect } from 'https://jslib.k6.io/k6chaijs/4.3.4.0/index.js';
import http from 'k6/http';

export default function testSuite() {
  describe('Basic API test', () => {
    let response = http.get("https://test-api.k6.io/public/crocodiles")
    expect(response.status, "API status code").to.equal(200);
  })
}
```

</CodeGroup>

If you are familiar with k6, this is similar to using the built-in `group` and `check` functionality but with different names.

When you run this test with `k6 run mytest.js` the result should look similar to this:

```
█ Basic API test
  ✓ API status code: expected 200 to equal 200
```

This basic example is not very exciting because the same result can be achieved with `group` and `check`, so let's move on to more interesting examples.

### Chain of assertions

When writing integration tests and performance test, it's often necessary to execute conditional checks. For example, you may want to inspect the JSON body only when the http response is 200. If it's 500, the body is not relevant and should not be inspected. 

Unlike `check()`, when `expect()` fails, it stops the execution of the following assertions in the entire `describe()`group.


<CodeGroup labels={[]}>

```javascript
import http from 'k6/http';
import { describe, expect } from 'https://jslib.k6.io/k6chaijs/4.3.4.0/index.js';

export default function testSuite() {

  describe('Fetch a list of public crocodiles', () => {
    let response = http.get("https://test-api.k6.io/public/crocodiles")

    expect(response.status, "response status").to.equal(201)
    expect(response).to.have.validJsonBody()
    expect(response.json().length, "number of crocs").to.be.above(4);
  });
}
```

</CodeGroup>

The above script should result in the following being printed after execution:

```
█ Fetch a list of public crocodiles
  ✓ response status is 200.
  ✓ https://test-api.k6.io/public/crocodiles has valid json response
  ✓ number of crocs is greater than 5
```

When the status code isn't 200, the remaining two calls to `expect()` are ommitted and the the result looks like this.

```
█ Fetch a list of public crocodiles
  ✗ response status: expected 502 to equal 200
```

More advanced examples can be found in the examples section [TODO](/examples/functional-testing)


Below we 

## Plugins


## Full API documentation

The full documented in available on Chai's oficial website at https://www.chaijs.com/api/bdd/

Below we 

| Function | Description |
| -------- | ----------- |
| [describe(name, function)](/javascript-api/jslib/expect/describe-name-function)  | Entry point for creating tests. |
| [expect(value)](/javascript-api/jslib/expect/expect-value)  | expect(value) sets the value to be used in comparison by the next function in the chain |



