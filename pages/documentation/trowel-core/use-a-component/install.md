---
title: Install a trowel component
permalink: /documentation/trowel-core/use-a-component/install/
---


## Install Trowel
### Download
A *Trowel component* cannot be compiled without Trowel installed into your scss file. So you need first to install Trowel-core which is the scss algorithm making every *Trowel component* working.

You can download Trowel via command line with npm, yarn or bower as shown below.

```sh
# npm
npm install trowel-core --save

# yarn
yarn add trowel-core

# bower
bower install trowel-core --save
```

You can also download a ZIP [clicking here](https://github.com/Trowel/Trowel/archive/master.zip).


### Import the library
Once downloaded, import it at the top of your main `scss` file :

```scss
// main.scss
@import 'path/to/your/dependencies/trowel-core/src/trowel';
```

## Install a *Trowel Component*
Trowel-core installed, we are now good to go and we can implement a *Trowel Component*.

Download the *Trowel Component* you want to use, and import it after `trowel-core` into your main `scss` file.

The download and the file to import depends on each library, but for the official *FriendsOfTrowel* components, it always respects the same pattern.

```scss
// main.scss
@import 'path/to/your/dependencies/trowel-core/src/trowel';

// the trowel component we want to use
@import 'path/to/your/dependencies/trowel-button/trowel-buttons/src/scss/buttons'
```

{% include page-control.html href="/documentation/trowel-core/use-a-component/config/" label="Set your global configuration" %}
