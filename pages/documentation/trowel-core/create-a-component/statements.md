---
title: Write statements
permalink: /documentation/trowel-core/create-a-component/statements/
---

In CSS we call *statement* a rule set comprising a selector followed by a declaration block.

With Trowel we have to write statements in a new way to make `Trowel variable` pattern working. Everything turns around a single mixin called `statement()`.

## How works the `statement()` mixin

```scss
// button selector object intiated
$btn--selector: selector(new 'btn');

// example of statement
@include statement($btn--selector, (
  ('display', inline-block),
  ('background-color', $button--background-color),
));
```

As you can see above, we first create a selector object and then we use the `statement()` mixin. This mixin accept 2 parameters :

* The selector object variable initiated before (see [previous section](/documentation/trowel-core/create-a-component/selector-object/)).
* A list of declarations.

But well, how do works declaration ?

## Write a declaration
A declaration is a list with 2 required parameters and third optional.

* 1st parameter : the css property that you want to generate
* 2nd parameter : the static value or `Trowel variable` you want to bind to the property
* 3rd parameter (optional) : a list of flags you want to staticaly set to the Trowel variable

```scss
// button selector object intiated
$btn--selector: selector(new 'btn');

// the trowel variable used in the statement
$button--background-color: (
    'default': rgb(255, 0, 0),
    '-primary': rgb(0, 255, 0)
);

// example of statement
@include statement($btn--selector, (
  ('display', inline-block),
  ('background-color', $button--background-color),
  ('cursor', not-allowed, ('[disabled')),
));

// css generated
.btn {
    display: inline-block;
    background-color: rgb(255, 0, 0);
}

.btn--primary {
    background-color: rgb(0, 255, 0);
}

.btn[disabled] {
    cursor: not-allowed;
}
```

We are almost done ! There is one pattern which is awesome in Sass that we wanted in Trowel : *mixins*

{% include page-control.html href="/documentation/trowel-core/create-a-component/mixins/" label="Define a trowel mixin" %}
