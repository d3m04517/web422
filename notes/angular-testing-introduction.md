---
title: Angular Testing Introduction
layout: default
---

## Introduction to Angular Testing

Throughout our discussion of Angular, we have largely been ignoring the test features that have been made available to us.  For example, we have opted to ignore the spec files and "skip tests" using the "-st" command when creating a "new" application (skipping the creation the spec files altogether, as well as excluding the end-to-end (e2e) testing functionality).

However, running e2e tests & unit testing is an [extremely important](https://www.agiletestingframework.com/atf/testing/unit-testing/) part of the Agile development cycle.  For example, unit / e2e testing:

* Guards against changes that break existing code (“regressions”).

* Clarifies what the code does both when used as intended and when faced with deviant conditions.

* Reveals mistakes in design and implementation. Tests shine a harsh light on the code from many angles. When a part of the application seems hard to test, the root cause is often a design flaw, something to cure now rather than later when it becomes expensive to fix.

So, to take full advantage of all the built in testing functionality that Angular provides, we will be primairly referring to the excellent [documentation](https://angular.io/guide/testing) (re-printed here with extra comments where applicable).

<br>

### Tools and technologies

You can write and run Angular tests with a variety of tools and technologies. This guide describes specific choices that are known to work well.

<table width="100%">
  <colgroup><col width="20%">
  
  <col width="80%">
  
  </colgroup><tbody><tr>
    <th>
      Technology
    </th>
    <th>
      Purpose
    </th>
  </tr>
  <tr style="top">
    <td style="vertical-align: top">
      Jasmine
    </td>
    <td>
<p>      The <a href="http://jasmine.github.io/2.4/introduction.html">Jasmine test framework</a>
provides everything needed to write basic tests.
It ships with an HTML test runner that executes tests in the browser.</p>
    </td>
  </tr>
  <tr style="top">
    <td style="vertical-align: top">
      Angular testing utilities
    </td>
    <td>
<p>      Angular testing utilities create a test environment
for the Angular application code under test.
Use them to condition and control parts of the application as they
interact <em>within</em> the Angular environment.</p>
    </td>
  </tr>
  <tr style="top">
    <td style="vertical-align: top">
      Karma
    </td>
    <td>
<p>      The <a href="https://karma-runner.github.io/1.0/index.html">karma test runner</a>
is ideal for writing and running unit tests while developing the application.
It can be an integral part of the project's development and continuous integration processes.
This guide describes how to set up and run tests with karma.</p>
    </td>
  </tr>
  <tr style="top">
    <td style="vertical-align: top">
      Protractor
    </td>
    <td>
<p>      Use protractor to write and run <em>end-to-end</em> (e2e) tests.
End-to-end tests explore the application <em>as users experience it</em>.
In e2e testing, one process runs the real application
and a second process runs protractor tests that simulate user behavior
and assert that the application respond in the browser as expected.</p>
    </td>
  </tr>
</tbody></table>

<br>

### Setup

Fortunately, since we have been making exclusive use of the Angular CLI to create "new" apps for us, everything we need to get started testing our software is already included.  

For example, if we create a "new" app with the following command:

```
ng new myApp --routing
```

We will get a new Angular application with "routing" enabled out of the box - this isn't anything new.  However, until now we haven't discussed a few of the key files that are added **outside** the src/app directory, including; karma.conf.js, protractor.conf.js, app.component.spec & the e2e folder (below).

For now, let's see how we can perform our first, simple unit test:

1. Open the file src/app/app.component.spec.ts & comment out all of the code (we will be coming back to this later).

2. Create a new file called `1st.spec.ts` in the application root folder, `src/app/`

  NOTE: Tests written in Jasmine are called specs. **The filename extension must be** .spec.ts, the convention adhered to by karma.conf.js and other tooling.
  
3. Open the file and enter the following code:

```js
describe('1st tests', () => {
    it('true is true', () => {
        expect(true).toBe(true);
    });
});
```

4. Run the command `npm test` from the integrated terminal

  You should see some text output to the terminal, ie:
  
  ```
18 12 2017 23:02:29.790:WARN [karma]: No captured browser, open http://localhost:9876/
18 12 2017 23:02:29.800:INFO [karma]: Karma v1.7.1 server started at http://0.0.0.0:9876/
18 12 2017 23:02:29.801:INFO [launcher]: Launching browser Chrome with unlimited concurrency
18 12 2017 23:02:29.804:INFO [launcher]: Starting browser Chrome
...
```

You may or may not see the "No captured browser" warning - if your default browser opens to a "Karma" page, then everything is working as expected and no further action is required.  If you see the warning, simply open the browser to the address identified, ie: `http://localhost:9876/`

We can now safely keep the "Karma" page running and every time we save a change to our code (as before), we will see the compiled results tested and our results shown.

For example:

If we make a quick change in our 1st.spec.ts by changing the expectation from **true** to **false**, ie:

```js
describe('1st tests', () => {
    it('true is true', () => {
        expect(false).toBe(true);
    });
});
```

We can see that it fails with "Expected false to be true".  Before moving on, change the expection back to "true" so that we no longer see the error

<br>

### Test Debugging

Debug specs in the browser in the same way that you debug an application.

1. Reveal the karma browser window (hidden earlier).
2. Click the DEBUG button; it opens a new browser tab and re-runs the tests.
3. Open the browser's “Developer Tools” (Ctrl-Shift-I on windows; Command-Option-I in OSX).
4. Pick the "sources" section.
5. Open the 1st.spec.ts test file (Control/Command-P, then start typing the name of the file).
6. Set a breakpoint in the test.
7. Refresh the browser, and it stops at the breakpoint.

<br>

### Jasmine Syntax

Before we move on to test our first component, let's take a look at a couple of the functions that we used to perform our "1st tests" using Karma.  As we know from above, the functions

**describe()**

A test suite begins with a call to the global Jasmine function **describe** with two parameters: a **string** and a **function**. The string is a name or title for a spec suite - usually what is being tested. The function is a block of code that implements the suite.

**it()**

Calling the global Jasmine function **it** actually defines a "spec" which, like describe takes a **string** and a **function**. The string is the title of the spec and the function is the spec, or test. A spec contains **one or more** expectations that test the state of the code. An expectation in Jasmine is an assertion that is either true or false. A spec with all true expectations is a passing spec. A spec with one or more false expectations is a failing spec.

Note: "Describing" test suites and "specs" helps us to group tests and easily identify (or report on) 1 or more failing tests.

**expect()**

The **expect** function is used to build "Expections", by providing a value, called the actual. It is chained with a Matcher function (ie: `toBe()`), which takes the expected value.

**Matcher Functions**

Jasmine comes with the following "Matcher Functions" that we can use to test our code.  Please note however, that it is also possible to create [custom matchers](https://jasmine.github.io/2.4/custom_matcher.html) when required as well.

### toBe()

```js
it("The 'toBe' matcher compares with ===", function () {
    var a = 12;
    var b = a;

    expect(a).toBe(b);
    expect(a).not.toBe(null);
});
```

### toEqual()

```js
it("works for simple literals and variables", function () {
    var a = 12;
    expect(a).toEqual(12);
});

it("should work for objects", function () {
    var foo = {
        a: 12,
        b: 34
    };
    var bar = {
        a: 12,
        b: 34
    };
    expect(foo).toEqual(bar);
});
```

### toMatch()

```js
it("The 'toMatch' matcher is for regular expressions", function () {
    var message = "foo bar baz";

    expect(message).toMatch(/bar/);
    expect(message).toMatch("bar");
    expect(message).not.toMatch(/quux/);
});
```

### toBeDefined()

```js
it("The 'toBeDefined' matcher compares against `undefined`", function () {
    var a = {
        foo: "foo"
    };

    expect(a.foo).toBeDefined();
    expect(a.bar).not.toBeDefined();
});
```

### toBeUndefined()

```js
it("The `toBeUndefined` matcher compares against `undefined`", function () {
    var a = {
        foo: "foo"
    };

    expect(a.foo).not.toBeUndefined();
    expect(a.bar).toBeUndefined();
});
```

### toBeNull()

```js
it("The 'toBeNull' matcher compares against null", function () {
    var a = null;
    var foo = "foo";

    expect(null).toBeNull();
    expect(a).toBeNull();
    expect(foo).not.toBeNull();
});
```

### toBeTruthy()

```js
it("The 'toBeTruthy' matcher is for boolean casting testing", function () {
    var a, foo = "foo";

    expect(foo).toBeTruthy();
    expect(a).not.toBeTruthy();
});
```

### toBeFalsy()

```js
it("The 'toBeFalsy' matcher is for boolean casting testing", function () {
    var a, foo = "foo";

    expect(a).toBeFalsy();
    expect(foo).not.toBeFalsy();
});
```

### toContain()

```js
it("works for finding an item in an Array", function () {
    var a = ["foo", "bar", "baz"];

    expect(a).toContain("bar");
    expect(a).not.toContain("quux");
});

it("also works for finding a substring", function () {
    var a = "foo bar baz";

    expect(a).toContain("bar");
    expect(a).not.toContain("quux");
});
```

### toBeLessThan()

```js
it("The 'toBeLessThan' matcher is for mathematical comparisons", function () {
    var pi = 3.1415926,
        e = 2.78;

    expect(e).toBeLessThan(pi);
    expect(pi).not.toBeLessThan(e);
});
```

### toBeGreaterThan()

```js
it("The 'toBeGreaterThan' matcher is for mathematical comparisons", function () {
    var pi = 3.1415926,
        e = 2.78;

    expect(pi).toBeGreaterThan(e);
    expect(e).not.toBeGreaterThan(pi);
});
```

### toBeCloseTo()

```js
it("The 'toBeCloseTo' matcher is for precision math comparison", function () {
    var pi = 3.1415926,
        e = 2.78;

    expect(pi).not.toBeCloseTo(e, 2);
    expect(pi).toBeCloseTo(e, 0);
});
```

### toThrow()

```js
it("The 'toThrow' matcher is for testing if a function throws an exception", function () {
    var foo = function () {
        return 1 + 2;
    };
    var bar = function () {
        return a + 1;
    };
    var baz = function () {
        throw 'what';
    };

    expect(foo).not.toThrow();
    expect(bar).toThrow();
    expect(baz).toThrow('what');
});
```

### toThrowError()

```js
it("The 'toThrowError' matcher is for testing a specific thrown exception", function () {
    var foo = function () {
        throw new TypeError("foo bar baz");
    };

    expect(foo).toThrowError("foo bar baz");
    expect(foo).toThrowError(/bar/);
    expect(foo).toThrowError(TypeError);
    expect(foo).toThrowError(TypeError, "foo bar baz");
});
```

### Manually failing a spec with 'fail'

```js
var foo = function (x, callBack) {
    if (x) {
        callBack();
    }
};

it("should not call the callBack", function () {
    foo(false, function () {
        fail("Callback has been called");
    });
});
```




