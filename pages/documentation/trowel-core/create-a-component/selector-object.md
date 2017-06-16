---
layout: documentation
type: trowel
title: Create a selector object
library: trowel/trowel
permalink: /documentation/trowel-core/create-a-component/selector-object/
affix: true
---

Before writing blocks of property and values, we have to set up selectors that will target DOM elements.

## What is a selector object
Usually selectors are just strings. But with Trowel we need to be able to add modifiers, tags, media-queries... to our selector in a dynamic way for making *Trowel variables* possible.

That's why selectors are objects, this way we can set/get/append properties to our selectors in order to print it out dynamically depending of Trowel variables' flags.

But objects do not exists in sass : selector object are in facts maps with as key what we call a property (and as a value... its value). Those selectors objects are created and manipuled with the function `selector()`.

## Create our first selector object
To initiate a selector object, we just require to set the *block* string of our selector (remember we are strongly committed to the BEM philosophy).

We will -as you can see in the snippet below- use the `selector()` function with as parameter `new` and then a string that will be our selector's block.

```scss
// initiate a selector object
$btn--selector: selector(new 'btn');

// it equals in css to :
.btn { ... }
```

You can also create a new selector object with a specific syntax settings. It will overrule the syntax settings from the `$trowel-config` variable for this selector and its descendant.

```scss
// selector syntax options
$btn--syntax: (
  'prefix': 'c',
  'separator': (
    'prefix': '-',
    'element': '__',
    'modifier': '--',
  ),
);

// initiate a selector object
$btn--selector: selector(new 'btn' with $button--syntax);

// it equals in css to :
.c-btn { ... }
```

Notice that selectors initiated are only class. It cannot be id's or tag elements. Tag elements can be added to the class via flags.

## Extend our selector object
Our selector object is initiated and we can now extend it by adding properties that will specify more it.

To set or append properties to the selector, we once again have to use the function `selector()`, and depending of its parameter, the function will return the selector object with new or edited properties.

To understand how works selectors objets and the function `selector()`, try some of the functions listed below and then debug your selector variable to see how it looks like

### The list of the extensions possible
Let suppose we have a `$btn--selector` variable where we had already initiated a selector object with the function `selector(new 'btn')`.

In the table below we list all the possibilty to extend this selector object. Keep in mind that the `selector()` must be stocked into a variable.

| Function                                                                    | Role                                                                                                                                         |
| --------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| `selector(set $btn--selector prefix 'd')`                                   | set the selector prefix                                                                                                                      |
| `selector(set $btn--selector prefix-separator '-')`                         | set the selector prefix separator                                                                                                            |
| `selector(set $btn--selector block 'btn')`                                  | set the selector block following BEM philosophy                                                                                              |
| `selector(set $btn--selector tag 'a')`                                      | add to the selector an html tag to specify                                                                                                   |
| `selector(set $btn--selector pseudo-classes ('hover', 'focus'))`            | add to the selector a list of pseudo-classes to specify                                                                                      |
| `selector(append $btn--selector pseudo-class 'hover')`                      | add to the selector a single pseudo-class to specify                                                                                         |
| `selector(set $btn--selector element 'icon')`                               | add to the selector class an element (according to the BEM philosophy)                                                                       |
| `selector(set $btn--selector element-separator '__')`                       | set the selector element separator                                                                                                           |
| `selector(set $btn--selector modifiers ('primary', 'secondary'))`           | add to the selector class a list of modifiers (according to the BEM philosophy)                                                              |
| `selector(append $btn--selector modifier 'primary')`                        | add to the selector class a single modifier (according to the BEM philosophy)                                                                |
| `selector(set $btn--selector modifier-separator '--')`                      | set the selector modifier separator                                                                                                          |
| `selector(set $btn--selector modifiers-element ('lg', 'sm'))`               | add to the selector class a list of modifiers-element (according to the BEM philosophy)                                                      |
| `selector(append $btn--selector modifier-element 'primary')`                | add to the selector class a single modifier-element (according to the BEM philosophy)                                                        |
| `selector(set $btn--selector attributes ('disabled', 'title^="disabled"'))` | add to the selector a list of attributes to specifiy                                                                                         |
| `selector(append $btn--selector attribute 'disabled')`                      | add to the selector a single attribute to specify                                                                                            |
| `selector(set $button--selector breakpoint 'sm')`                           | add to the selector a responsive breakpoint in order to generate a media-query bracing the selector based on the min-width of the breakpoint |
| `selector(set $button--selector pseudo-element 'before')`                   | add to the selector a pseudo element to specify                                                                                              |
| `selector(append $button--selector parent $button-group--selector as '>')`  | add to the selector an another parent selector written just before, and its relationship                                                     |

Some properties (pseudo-classes, modifiers, modifiers-element, attributes, parents) are collections, that means they can have several values. That is why there is two starting keywords for `selector()` function :
* `set` redefines the whole properties (you can write the value has a list or just a string if your collection is just one item)
* `append` add a new item to the propertie's collection.

Be aware that when the `selector()` function starts with the `append` keyword, the property name is written in singular (`'modifiers'` becomes `'modifier'`).

## Print selector
Even if most of the time we will use the mixin `statement()` to print out a selector, you may sometime to just generate the string from your selector object for flexibility.

It is possible with the function `print-selector()` as shown below

```scss
// we create a selector object
$btn--selector: selector(new 'btn');

// we print out the selector with the `print-selector()` function
#{print-selector($button--selector)} svg {
  vertical-align: middle;
}

// it will be compiled into :
.btn svg {
  vertical-align: middle;
}
```


We got our selectors, but we need now to implement our Trowel variables in order to make the Trowel sauce !

{% include page-control.html href="/documentation/trowel-core/create-a-component/statements/" label="Write statements" %}
