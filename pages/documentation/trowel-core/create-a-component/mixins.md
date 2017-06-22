---
title: Define a trowel mixin
permalink: /documentation/trowel-core/create-a-component/mixins/
---

In the `statement()` mixin, the declaration is the binding of one Trowel variable to a property. But if you want to make some calculation on the result of the variable, or if you want to apply its value to several properties you will have to make *Trowel mixins*.

A Trowel mixin is not a real Sass mixin. It is in fact a function that will be called anonymously in the statement.

## Call a mixin
We call a mixin in a declaration instead of writing property name as first parameter. Just prefix the mixin name by `'@include '` to call it.

```scss
$btn--background-color: (
  'default': rgb(255, 0, 0),
  '-primary': rgb(0, 255, 0),
);

// we call the mixin `btn--background-color-mixin` which will have variable passed `$btn--background-color`
@include statement($btn--selector, (
  ('@include btn--background-color-mixin', $btn--background-color),
));
```

The mixin will be called for each value found in the Trowel variable map binded in the declaration. So in the snippet above, the mixin `btn--background-color-mixin` will be called twice.


## Write your mixin
As we learned previously, Trowel mixins are in fact function and are called for each flag of the Trowel binded with.

So a Trowel mixin must have 2 params and no others :
* `$value` : The curent value from the call
* `$flags` : The list of flags corresponding the current value (it is a list because flags can be nested and Trowel need all the nested flags tree for the value)

Any Sass function must return something. In the case of *Trowel mixins*, it must return a list of declaration like we wrote in the `declaration()` mixin.

But this time the third argument is required and it must be the `$flags` parameter or a list that appened a new flag to the `$flags`. Check below for a clear example.

The `$flags` param is required because without it Trowel coudn't use all the flags from the Trowel variable, and coudn't print the value in the good selector.

```scss
// This is a Trowel mixin
@function btn--background-color-mixin ($value, $flags) {
  $default-value: $value;
  $state-value: darken($value, 10%);

  // Here the mixin return the list of declarations
  @return (
    ('background-color', $default-value, $flags), // do not forget the 3rd param with $flags
    ('background-color', $state-value, append($flags, ':hover')), // use the native append() method to add a new flag to the list
  );
}
```

The mixin above is good to go ! Let see what it looks like in a full snippet

```scss
// Trowel variable
$btn--background-color: (
  'default': rgb(255, 0, 0),
  '-primary': rgb(0, 255, 0),
);

// Selector object initiated
$btn--selector: selector(new 'btn');

// Trowel Mixin
@function btn--background-color-mixin ($value, $flags) {
  $default-value: $value;
  $state-value: darken($value, 10%);

  @return (
    ('background-color', $default-value, $flags),
    ('background-color', $state-value, append($flags, ':hover')),
  );
}

// Statement
@include statement($btn--selector, (
  ('@include btn--background-color-mixin', $btn--background-color), // Mixin implementation
));

// The CSS compiled
.btn {
  background-color: rgb(255, 0, 0);
}

.btn:hover {
  background-color: rgb(204, 0, 0);
}

.btn--primary {
  background-color: rgb(0, 255, 0);
}

.btn--primary:hover {
  background-color: rgb(0, 204, 0);
}
```

## Usage of the `value()` function
In a mixin you may need to access of an another value than variable binded to the mixin in the declaration. The `value()` is here for this, it will return you the specific value of a Trowel variable for specific list of flags.

The `value()` function has two parameters :
* a Trowel variable
* a list of flags (optional)

If you do not specify a list of flags as second parameter, the `value()` function will return the default value or `null` if there is no default value.

```scss
$btn--background-color: (
  'default': rgb(255, 0, 0),
  '-primary': (
    'default': rgb(0, 255, 0),
    '[disabled': rgb(0, 0, 255),
  )
);

$var: value($btn--background-color); // returns rgb(255, 0, 0)
$var: value($btn--background-color, ('-primary')); // returns rgb(0, 255, 0)
$var: value($btn--background-color, ('-primary', '[disabled')) // returns rgb(0, 0, 255);
```

{% include page-control.html href="/documentation/trowel-core/create-a-component/generator/" label="Use the component generator" %}
