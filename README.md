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
5. [Comments (Sassdoc)](#comments)
6. [Variables](#variables)
7. [Handling breakpoints: Element queries](#handling-breakpoints)
8. [Categorizing CSS-Rules](#categorizing-css)
9. [CSS-Stuff](#css-stuff)
10. [Other](#other)



<a name="folderstructure">General Principles</a>
-----------

Having a good structure for your development process is so important and the basic of all. You should pay attention that the base is mostly the same in every project – it will help you and your co-workers to incorporate easily in new projects. I will show you my personal structure (based on SMACSS):

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

|- application.scss

stylesheets/
|- application.css
```

<a name="structure-a-module">Structure a module</a>
-----------

A structure for modules? Is that not too much? No, I don’t think so. A module can have so many different types or become very complex, a well planned structure will help you a lot.

```sass
@charset "UTF-8";

// Config
$button-bgcolor: $blue;
$button-fontcolor: $white;

// Base
.m-button {}
.m-button--primary {}
.m-button--secondary {}
```

#### The module has ~~three~~ two different sections
* At first we have a area for Config. There I define specific colors or other stuff for configuration like colors.
* Now the main part of the file, Base. Writing all the styles here for all types of the module
* ~~The footer called States. Here I import the states for the current module. Not more.~~


<a name="importing-files">Importing files</a>
-----------

In a project there are so many partials. I don’t like to include files in other files. I prefer to have one single file like `application.scss` and there I include all other. For this I created a structure like in this example:

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
* For each section declare `@import` only once
* Remove file-ending


### Don't blow up your files
If you have a big project with much modules and variants, then you come to the point to make thoughts about the current structure. You blow up your file with a bunch of code and it's hard to maintain it.

Because of this we need a small improvement of the structure and add some folders if you noticed, that your module need to much lines of code. Then make more small partials and include it in one mainfile for this module.

```sass
// Base
@import "_base/_config",
        "_base/_presets",
        "_base/_headings";

// Layouts
@import "_layouts/_l-default";

// Modules
@import "_modules/_forms/_m-forms";
```

In the `_m-forms.scss` you must import all the form partials, which are in your `_form` folder. The structure can look like this in one in your file:

```sass
// Partials
@import "_m-form-base",
        "_m-form-two-one-small",
        "_m-form-row",
        "_m-form-with-prefix";
```

**You must decide which one do you prefer. I can't say which one do you should use, because of my decision is based on the current project.**


<a name="comments">Comments (Sassdoc)</a>
-----------

To documentate the code is so important everytime. If you take a look at the environment after a few time, you ask yourself what did you write. It's hard to follow without comments and there I give you one advice, specially for Sass: *use [Sassdoc](https://github.com/SassDoc/sassdoc)* It's an awesome tool to create a solid documentation for your variables, mixins, functions etc. You must only write much more comments, but you gain a lot of it.

```sass

/**
 * Get the width of a column
 *
 * @author Tim Hartmann
 * @group addons
 *
 * @param {number} $col - define which column do you need
 * @param {number} $max-cols (12) - maximum columns
 *
 * @example scss
 * col(4);
 * // 25%
 */
@mixin col($col, $max-cols: 12) {
  width: (100%/$max-cols)*$col;
}

```
*At the first line of your comment, give a small description about the snippet. It helps a lot to have a quick overview.*

`@author`: Declare the person, which wrote this snippet. <br/>
`@group`: Grouping snippets are great to give it more structure <br/>
`@param`: Possible parameters for this mixin / function and write the type of each <br/>
`@example`: Write a quick example how you can use it


**[Example](http://scss-components.com/sassdoc/)** <br/>
**[Sassdoc Documentation](https://github.com/SassDoc/sassdoc)**


<a name="naming-conventions">Naming conventions (BEM/SMACSS)</a>
-----------

### Block, Element, Modifier / Scalable and modular architecture for CSS
I love both ways. However, SMACSS is a must read for every frontend-developer who wants to write scalable front-ends. Jonathan Snook is the author and he wrote down everything about his experience in a book. Combining these two methods is the best way for me to handle naming conventions.

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

At first, we create a palette for globally usaged things like colors in the `_config.scss` file. If you have some specific variables, then put them in the file for the module. Readability is very important and because of this I use separators in variable-naming: a **hyphen**.

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

Element Queries? What do you write, Tim? Uhh, Element Queries with Sass are so much better than media queries. Do you know how it is to have all your specific responsive styles in an own file? In my opinion, that is crap. If you write modules you should declare the responsive behavior of it there. **Not in another file.**

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

It is not important for the performance to have only one declaration of a media query. Thus, we can use it like **Element Queries.** Read more about this in [a blog post](http://helloanselm.com/2014/web-performance-one-or-thousand-media-queries/) by Anselm Hannemann who had the same question.


<a name="categorizing-css">Categorizing CSS-Rules</a>
-----------

If I write CSS / Scss I am really consistent and want to set rules for all kinds of stuff. Thus, I have a couple of rules for categorizing my styles.

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
  * `margin`, `padding`, `display`, etc.
* Border & Background
  * `border`, `border-radius`, `background-color` and so on
* Text
  * `color`, `font-size`, `text-transform`, etc.
* Other stuff
  * `animations`, `transform`'s, ...


<a name="css-stuff">CSS-Stuff</a>
-----------

### Avoid IDs
IDs are really hard. They are too specific and mostly necessary for JavaScript hooks. The better way is to use classes.

### Avoid `!important`
If you need the `!important` declaration something is seriuosly wrong with your CSS architecture.
You need this only if you want really want to overwrite styles. If your code structure is clean you won't need this. It’s like using sledgehammer to crack a nut.

### Avoid child selectors
Why do I avoid child selectors? Isn't it a good CSS-Feature, is it? No, I don’t think so.
When I started with coding, I used it quite often. However, the more I learned about (S)CSS the more I noticed that it makes a module too specific. It depends on the structure of your HTML and can make it quite difficult to change its structure afterwards. **Use classes!**

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

* Overall two whitespaces instead of tabs for indentation
* One whitespace between elements and braces
* Line breaks between styles and new elements
* Line breaks after closing braces
* Use hex codes instead of RGB codes for colors
* Convert hex codes to RGB with Sass
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
