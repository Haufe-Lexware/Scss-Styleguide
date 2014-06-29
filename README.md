Scss-Styleguide
===============

Way of thinking
---------------
* [OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/) / [SMACSS](http://smacss.com/)
* Progressive Enhancement
* Responsive Development: Mobile first

Table of Contents
-----------------

1. [Folder Structure](#folderstructure)
2. [Formatting](#structure-a-module)
3. [Importing files](#importing-files)
4. [Naming conventions (BEM/SMACSS)](#naming-conventions)
5. [Variables](#variables)
6. [Handling breakpoints: Element queries](#handling-breakpoints)
7. [Categorizing CSS-Rules](#categorizing-css)
8. [CSS-Stuff](#css-stuff)
9. [Other](#other)



<a name="folderstructure">General Principles</a>
-----------

Having a good structure for your development-process is so important and the basic of all. You should pay attention that the base is mostly equal in each project – it will help you and your co-workers to incorporate easy in new projects. I will show you my personal structure (based on SMACSS):

```sass
scss/
|- _base/
|  |- _config.scss
|  |- _presets.scss
|  |- _headings.scss
|  |- ...

|- _layouts/
|  |- _l-base.scss
|  |- _l-grid.scss

|- _modules/
|  |- _m-buttons.scss
|  |- _m-tabs.scss

|- _states/
|  |- _s-buttons.scss
|  |- _s-tabs.scss
|- application.scss

stylesheets/
|- application.css
```

<a name="structure-a-module">Structure a module</a>
-----------

A structure for module? It’s not too much? No I don’t think so. A module can has so much different types or become much complexity.

```sass
// Config
$button-bgcolor: $blue;
$button-fontcolor: $white;

// Base
.m-button {}
.m-button--primary {}
.m-button--secondary {}

// States
@import "../_states/_s-buttons";
```

#### The module has three different sections
* At first we have a area for Config. There I define specific colors or other stuff for configuration like colors.
* Now the main part of the file, Base. Writing all the styles here for all types of the module
* The footer called States. Here I import the states for the current module. Not more.


<a name="importing-files">Importing files</a>
-----------

In a project there are so many partials. I don’t like to include files in other files. I prefer to have one single file like application.scss  and there I include all other. For this I created a structure like in this example:

```sass
// Base
@import "_base/_config",
        "_base/_presets",
        "_base/_headings";

// Layouts
@import "_layouts/_l-default";

// Modules
@import "_modules/_m-buttons",
        "_modules/_m-tabs";
```

#### Short:
* Structure in base, layouts and modules
* For each section only declare @import  once
* Remove file-ending


<a name="naming-conventions">Naming conventions (BEM/SMACSS)</a>
-----------

### Block, Element, Modifier / Scalable and modular architecture for CSS
I love both methods. Above all SMACSS is a must read for every frontend-developer, which want to write scalable frontends. Jonathan Snook is the author and he wrote all his experience in a little book. Combine these two methods is the best way for me to handle naming.

```sass
// Layouts
// Prefix "l-"
.l-default {}

// Modules
// Prefix "m-"
.m-accordion {}

// Child element of accordion
// Seperator: "__"
.m-accordion__trigger {}

// Modifier of accordion
// Seperator: "--"
.m-accordion--plain {}

// States
// Mostly with prefix like "is-"
.is-active {}
.is-hidden {}
```


<a name="variables">Variables</a>
-----------

At first we create a palette for globally usage like colors. We define it in the _config.scss . If we have some specific variables, then we put it in the file for the module. Readability is so important and because of this I use separators in variable-naming: a hyphen.

```sass
// Colors (global)
$white: #fff;
$blue: #1675d6;
$red: #e8402a;

// Specific naming
$button-bgcolor: $blue;
$button-fontcolor: $white;
```


<a name="handling-breakpoints">Handling Breakpoints: Element Queries</a>
-----------

Element Queries? What do you write, Tim? Uhh, Element Queries with Sass are so much better than the mediaqueries. Do you know how it is too have all your specific responsive styles in an own file? This is crappy I think. You write modules, then you should declare the responsive behaviour of it there. **Not in another file.**

```sass
// Scss
.m-navigation {
  &:after {
    @media screen and (min-width: 767px) {
      content: '';
      display: inline-block;
      width: 100%;
    }
  }
  li {
    display: block;

    @media screen and (min-width: 767px) {
      float: left;
    }
  }
}

// Generated CSS
@media screen and (min-width: 767px) {
  .m-navigation:after {
    content: '';
    display: inline-block;
    width: 100%;
  }
}

.m-navigation li {
  display: block;
}

@media screen and (min-width: 767px) {
  .m-navigation li {
    float: left;
  }
}
```

It’s not important for the performance that we have only one declaration of the mediaquery. Because of this we can use it more like **Element Queries.** Read more about this situation at [a blogpost](http://helloanselm.com/2014/web-performance-one-or-thousand-media-queries/) by Anselm Hannemann, which had the same question.


<a name="categorizing-css">Categorizing CSS-Rules</a>
-----------

If I write CSS / Scss I am really consistent and want to set rules for all kind of stuff. Because of this I have rules for categorizing my styles.

```sass
// Element-Base
.element {
  margin: 10px;
  padding: 10px;

  border: 1px solid red;
  background: blue;

  color: white;
  font-size: 16px;

  -webkit-transform: translate3d(0,0,0);
}
```

* Box
  * margin, padding, display, etc.
* Border & Background
  * border, border-radius, background-color, etc.
* Text
  * color, font-size, text-transform, etc.
* Other stuff
  * animations, transforms, etc.


<a name="css-stuff">CSS-Stuff</a>
-----------

### Avoid ID’s
ID’s are really hard. They are to specific and mostly necessary for JS-Hooks. The better way is to use classes.

### Avoid !important
If you need this, than goes something wrong at your CSS-architecture, because you need this only if you want to overwrite styles. If your structure is clean, than you will don’t need this. It’s like to shot with a cannon to sparrows.

### Avoid child-selector
Why I avoid child-selector? It is yet a good CSS-Feature? No I don’t think so. At my start I used it often, but more and more I learned that it makes the module to specific. It depends on the structure of your html, but what is if the order will be changed? **Use classes!**

```sass
// Bad example
// Child-selector
.m-tabs > li {}

// Good example
// Classname
.m-tabs .m-tabs__trigger {}

// Better example
// Less nesting
.m-tabs__trigger {}
```

<a name="other">Other</a>
-----------

* Overall two whitespaces instead tabs for distances
* One whitespace between element and brace
* Line-Break between styles and new element
* Line-Break after closed brace
* Using HEX instead RGB for colors
* Convert HEX to RGB with Sass
* bad example:
  * color: ```rgba(255,255,255,.3);```
* good example:
  * color: ```rgba(#000, .3);```

```sass
// Same styles for more than one element
// Separation with a line-break
.element,
.second-element {
  color: red;
}

.m-tabs {
  background: rgba($black, .3);
}
```