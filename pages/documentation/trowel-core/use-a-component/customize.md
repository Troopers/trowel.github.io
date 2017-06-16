---
layout: documentation
type: trowel
title: Customize a trowel component
library: trowel/trowel
permalink: /documentation/trowel-core/use-a-component/customize/
affix: true
---

Everything is set up, we can now customize our *Trowel Component* with the *Trowel variables*.

A *Trowel variable* is a simple scss variable provided by the *Trowel Component* you want to customize. Read the component documentation to be sure the variable you will redefine is a *Trowel variable*.

Imagine we installed the [official trowel button component](https://github.com/FriendsOfTrowel/Buttons). This component allow us to redefine the `$button--background-color`, which is the variable setting the background color of a button.

## Single value
The value can be a single value as usual for scss variables :

```scss
$button--background-color: rgb(255, 0, 0);

// compiled css
.btn {
  background-color: rgb(255, 0, 0);
}
```

The variable name is quitte obvious about its purpose, but it is only semantics. Trowel Component should provide explanation on the action of each variable.

## Null value
The `$button--background-color` can also be setted as `null`. This will just not generate declaration into the selector for the property binded to the variable.

## The flags concept
The `$button--background-color` can be transformed into a map. A map a type of variable which is an array matching keys to value.
In trowel variables, the keys are called *flags* and provide a context its value.

Flags follow simple pattern recognized by trowel-core to generate the property value to the good context.
Here is all the flags types

## Pseudo-classes flags
You can specify a value with a specific pseudo-class by writting a flag starting by `':'` and then the pseudo-class you want to target like `'hover'`, `'active'`, `'first-child'` or `'nth(1n + 3)'`.

```scss
$button--background-color: (
  ':hover': rgb(255, 0, 0),
);

// compiled css
.btn:hover {
  background-color: rgb(255, 0, 0);
}
```

Trowel will not cover you back, if you write a pseudo-class which is not valid, no error will be raised at the scss compilation.

## Tags flags
Tags flags allow to specify a value when the selector is applied to a specific html tag. Tags flags must start by `'<'` and then the tag's name like : '<input', '<button', '<select' or '<div'.

```scss
$button--background-color: (
  '<a': rgb(255, 0, 0),
);

// compiled css
a.btn {
  background-color: rgb(255, 0, 0);
}
```


## Responsive breakpoints flags
Flags also allow to specify a value when the viewport has a min-width equaled to a responsive breakpoint setted into the trowel configuration. This kind of flag starts with `'@'` and then a responsive breakpoint written into the `$trowel-config` variable.

```scss
// configuration
$trowel-config: (
  'breakpoints': (
    'sm': (
      'min': 36rem,
      'max': 74.9999rem,
    ),
    'md': (
      'min': 75rem,
    )
  ),
);

$button--background-color: (
  '@sm': rgb(255, 0, 0),
);

// compiled css
@media (min-width: 36rem) {
  .btn {
    background-color: rgb(255, 0, 0);
  }
}
```


## Modifiers flags
Following BEM philosophy, you can write a flag with to specify a value when selector has a modifier. To write a modifier flag, it must begin with `'-'` and then the string of the modifier name like `'-pod'`.

See the [Set you global configuration](./2.2-set-your-global-configuration.md) section to set the modifier separator syntax.

```scss
$button--background-color: (
  '-primary': rgb(255, 0, 0),
);

// compiled css
.btn--primary {
  background-color: rgb(255, 0, 0);
}
```

If the variable is binded to an element selector like the variable `$button-icon--background-color` below it will generate a selector where the modifier is written on the block selector and not on the element :

```scss
$button-icon--background-color: (
  '-primary': rgb(255, 0, 0),
);

// compiled css
.btn--primary .btn__icon {
  background-color: rgb(255, 0, 0);
}
```

## Element-modifiers flags
If you want a modifier directly written on the element and not on the block you can write a modifier flag with `'~'` instead of `'-'`.

```scss
$button-icon--background-color: (
  '~primary': rgb(255, 0, 0),
);

// compiled css
.btn__icon--primary {
  background-color: rgb(255, 0, 0);
}
```

## Attributes flags
Attributes flags allow to specify a value when the selector is applied to a specific html attribute. Attribute flags must start by `'['` and then the attribute name with optionaly its value like : '[disable' or '[href=^"#"'.

```scss
$button--background-color: (
  '[disabled': rgb(255, 0, 0),
);

// compiled css
.btn[disabled] {
  background-color: rgb(255, 0, 0);
}
```

## Flags collection
The *Trowel variable* maps can have as many flags/value as you want :

```scss
$button--background-color: (
  '-primary': rgb(255, 0, 0),
  '[disabled': rgb(0, 255, 0),
  '@sm': rgb(0, 0, 255),
);

// compiled css
.btn--primary {
  background-color: rgb(255, 0, 0);
}

.btn[disabled] {
  background-color: rgb(0, 255, 0);
}

@media (min-width: 36rem) {
  .btn {
    background-color: rgb(0, 0, 255);
  }
}
```

## The `'default' flag`
In the example below we setted the `background-color` for several context but we did not a context-less value. The `'default'` flag is here to set a value without adding more context.

```scss
$button--background-color: (
  'default': rgb(255, 0, 0),
  '-primary': rgb(0, 255, 0),
);

// compiled css
.btn {
  background-color: rgb(255, 0, 0);
}

.btn--primary {
  background-color: rgb(0, 255, 0);
}
```


## Nesting Flags
You can nest the map to add even more context to the values.

```scss
$button--background-color: (
  'default': rgb(255, 0, 0),
  '-primary': (
    'default': rgb(0, 255, 0),
    '@sm': rgb(0, 0, 255),
  ),
);

// compiled css
.btn {
  background-color: rgb(255, 0, 0);
}

.btn--primary {
  background-color: rgb(0, 255, 0);
}

@media (min-width: 36rem) {
  .btn--primary {
    background-color: rgb(0, 0, 255);
  }
}
```

Notice that you cannot nest a *responsive breakpoint flag* into an another *responsive breakpoint flag*, and a *tag flag* in a *tag flag*.

{% include page-control.html href="/documentation/trowel-core/create-a-component/" label="Create a trowel component" %}
