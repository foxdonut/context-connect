# context-connect

Helper to connect a context to composed functions.

## Function composition

Function composition is stringing together a series of functions, where the
result of calling one function is passed on to the next function.

Creating chains, or pipelines, of functions is a nice way to assemble
functionality from small, focused functions. Often, however, function
composition takes `f` and `g` and returns a function that first calls `g` and
then passes the result to `f`. I find it much more intuitive for the order in
which the functions are called to be the same as we read them, left-to-right.
A couple of useful tools for doing that are:

- [lodash flow](https://lodash.com/docs#flow)
- [functional-pipeline](https://github.com/bahmutov/functional-pipeline)

## Using a context

When chaining a series of functions, you might want to use a context object to
gather all the information from the functions. Each function takes the context
as a parameter and returns it as a result, having augmented it with the result
of its operation.

## Using context-connect

The problem with using a context is that some of the functions that you want to
use in your pipeline may not expect a context as a parameter, and/or return a
context as a result. Rewriting such functions, or wrapping them, is tedious.
This is where context-connect can help.

Say you have a pipeline of the following functions:

```
f1 f2 g f4
```

The `f1`, `f2`, and `f4` functions all expect and return the context. The `g`
function, however, accepts two parameters, `a` and `b`, and returns a result,
`c`. How can you connect `g` into the pipeline?

When you `require("context-connect")`, you get a function that accepts a series
of properties to take out of the context, the function that you want to call
with those values, and the property to use when putting the result back into the
context.

Continuing with the previous example, here is how you can assemble the
functions:

```js
var ctx = require("context-connect");
var _ = require("lodash");

_.flow(f1, f2, ctx("a", "b", g, "c"), f4);
```

If the function already accepts a context but returns a simple value instead of
the context, you can omit the properties to take out of the context.

Similarly, if you want to take properties out of the context to pass to the
function, but want to use the result as is, you can omit the result property.


