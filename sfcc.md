# UI Ecosystem - Sales Force Commerce Cloud
*Addendum to DEG UI Code Standards, Design Patterns, Frameworks & Tooling for SFCC Projects*

Unless otherwise mentioned, all the contents of the main DEG UI document apply to SFCC. This document only modifies the main DEG UI document.

## Table of Contents
  1. [Code Style Guide](#code-style-guide)
    - [HTML](#html)
    - [CSS](#css)
    - [Javascript](#javascript)
  2. [Design Patterns & Considerations](#design-patterns--considerations)
    - [Atomic Design](#atomic-design)
    - [Accessibility](#accessibility)
    - [Performance](#performance)
  3. [Frameworks](#frameworks)
    - [Skeletor](#skeletor)
    - [Pattern Lab](#pattern-lab)
  4. [Libraries](#libraries)
    - [DEGJS](#degjs)
    - [jQuery](#jQuery)
  5. [Tooling](#tooling)
    - [IDEs/Editors](#ides)
    - [CSS](#css-1)
    - [Javascript](#javascript-1)
    - [Task Runners](#task-runners)
    - [Visual Editors](#visual-editors)
    - [ALM/Version Control](#alm)
    - [Continuous Integration](#continous-integration)
    - [Testing](#testing)
    - [Reviews and Pull Requests](#pull-requests)


## Code Style Guide

### HTML
**Formatting & Syntax**
* Use tabs (4 spaces) for indentation.
* Nested elements should be indented once.
* Don’t omit optional closing tags (e.g. `</li>` or `</body>`).
* Typically there is no need to specify a type when including CSS and JavaScript files as text/css and text/javascript are their respective defaults.
* JavaScript files should be included at the bottom of a document whenever possible.

     __Bad Formatting__
    ```html
    <header class="header header--primary">
                <nav class="nav nav--primary">
            // ...
        </nav></header>
    ```

    __Good Formatting__
    ```html
    <header class="header header--primary">
        <nav class="nav nav--primary">
            // ...
        </nav>
    </header>
    ```


**HTML5 doctype**
* HTML5 (HTML syntax) is preferred for all HTML documents.
* Enforce standards mode and more consistent rendering in every browser possible with this simple doctype at the beginning of every HTML page: `<!DOCTYPE html>`.

**Semantics**
* Use semantic elements when possible. For example, use `<header>` elements for headers, `<p>` elements for paragraphs, `<button>` elements for buttons, etc.
* Using HTML according to its purpose is important for accessibility, SEO, reuse, and code efficiency.

    __Bad Semantics__
    ```html
    <div class="header header--primary">
        <div class="nav nav--primary">
            // ...
        </div>
    </div>
    ```

    __Good Semantics__
    ```html
    <header class="header header--primary">
        <nav class="nav nav--primary">
            // ...
        </nav>
    </header>
    ```

### CSS
**PostCSS**
* DEG utilizes [PostCSS](http://postcss.org/) to process CSS files and aims to write modern and future-proof CSS based on W3C specifications while avoiding the proprietary syntax of preproccesors whenever possible.
* [Skeletor](http://github.com/degdigital/skeletor) comes preconfigured with our preferred out-of-the-box PostCSS plugins, but developers are encouraged to add new plugins as the need arises on a per-project basis, while keeping the overall goals of future-proof CSS in mind.

**Organization**
* CSS should be organized into partials and follow DEG's modified Atomic CSS structure of Basics, Components, Templates, & Utilities. These partials will be processed using PostCSS and the available configuration options in Skeletor.

    ```
    css
    |-- basics/
    |   |-- buttons.css
    |   |-- headings.css
    |   |-- ...
    |-- components/
    |-- templates/
    |-- utilities/
    ```

**Formatting**
* Use tabs (4 spaces) for indentation
* Use ID selectors sparingly, if at all
* Use classes over generic element tags when possible
* When using multiple selectors in a rule declaration, give each selector its own line
* Properties should be organized in the most logical order
* When defining multiple properties, give each property its own line

    __Bad Formatting__
    ```css
    .selector{
            property:value; property:value; }
        .selector, .selector, .selector {
            // ...
        }
        #id {
          // ...
        }
    ```

    __Good Formatting__

    ```css
    .selector {
        property: value;
        property: value;
    }

    .selector,
    .selector,
    .selector {
        // ...
    }
    ```

**Pseudo BEM**
* [BEM](http://getbem.com/) is a CSS methodology and syntax, that helps achieve reusable components. BEM stands for Block Element Modifier and consists of:
    * __Block__: Standalone entity that is meaningful on its own.
    * __Element__: Parts of a block and have no standalone meaning. They are semantically tied to its block.
    * __Modifier__: Flags on blocks or elements. Use them to change appearance or behavior.
* DEG's approach is to follow the general guidelines of BEM and it's syntax, though it is not strictly enforced, which is why it's referred to as Pseudo BEM here and throughout this document. For full documentation on the BEM methodology, reference the [BEM website](http://getbem.com/).

**Variables**

* Use CSS variables for consistancy and maintainablity of styles.
* It is recommended that you use variables for color palettes, font properties, & timing functions. Additional variables may be created on an as needed basis.
* Namespace all variables with `--property-group`.
* Append a logical and easy to reference modifier to all variations: `-modifier`.
* If a logical scale can be applied, `-point-scale` can be used as the modifier. If no logical scale can be applied, use logical modifiers i.e. `-light`.
* Add a line break between different property group types.
* There are no standardized guidelines for mapping variables to other variables, but it is strongly encouraged that mappings are kept consistant and easy to reference.

    __Bad__
    ```css
    --blue: #005da8;
    --blue2: #00a0dd;
    --blue3: #acd5f8;
    --timing: .25s;
    --othertiming: 1s;
    ```

     __Good: Logicial Modifiers__
    ```css
    --color-blue: #005da8;
    --color-blue-light: #00a0dd;
    --color-blue-dark: #acd5f8;

    --timing-fast: .25s;
    --timing-slow: .1s;
    ```

     __Good: Point Scale__
    ```css
    --color-blue-10: #00a0dd;
    --color-blue-20: #005da8;
    --color-blue-30: #acd5f8;
    ```

**Nested Selectors**
* Following BEM or pseudo BEM should allow you to avoid unnecessary nesting. This creates CSS that is easier to maintain, less fragile, and smaller in size. When nesting does become needed, it should be kept as shallow as possible. A good rule of thumb is to nest selectors no more than three levels deep.

    When selectors become more deeply nested than three levels, you're likely writing CSS that is:
    * Strongly coupled to the HTML & fragile
    * Overly specific
    * Not reusable


* When nesting is required, nesting order should be based on specificity.
    1. __Properties applied to selector__: `property: value;`
    2. __Element modifiers__: `:before`, `:hover`, `&--modifier`
    3. __Elements__: `li`, `span`
    4. __Classes__: `.nested-class`

    __Bad Nesting Order__
    ```css
    .selector {
        element {
            // ...
        }
        property: value;
        property: value;
        .class {
            // ...
        }
        &:before {
            // ...
        }
    }
    ```

     __Good Nesting Order__
    ```css
    .selector {
        property: value;
        property: value;
        &:before {
            // ...
        }
        element {
            // ...
        }
        .class {
            // ...
        }
    }
    ```

**Breakpoints & Media Queries**
* __Write Mobile First CSS.__ Authoring mobile-first styles results in smaller, simpler, more maintainable code and is in line with DEG's stance on progressive enhancement.
* __Don’t use device dimensions to determine breakpoints.__ The device landscape is always changing, so today’s values might be moot even just a year down the road. The Web is inherently fluid, so it’s our job to create interfaces that look and function beautifully on any screen instead of in just a few arbitrary buckets.
* __Use `em`'s to define dimensions for media queries__. Avoid `px` and `rem` based units as `em`'s are the only units that [perform reliably across browsers](http://zellwk.com/blog/media-query-units/).

    __Bad__

    ```css
    .column {
        float: left;
        width: 50%;
    }
    @media all and (max-width: 800px) {
        .column {
            float: none;
            width: auto;
        }
    }
    ```
    __Good__

    ```css
    @media all and (min-width: 50em) {
        .column {
            float: left;
            width: 50%;
        }
    }
    ```

**Vendor Prefixes**
* Avoid using vendor prefixes within your authored CSS. [Autoprefixer](https://github.com/postcss/autoprefixer) is available within [Skeletor](https://github.com/degdigital/skeletor) and should be configured to apply vendor prefixes based on a projects browser support through the build process.

**Javascript Hooks & State Classes**
* Avoid binding to the same class in both your CSS and JavaScript.
* Depending on your specific needs, we recommend creating Javascript hooks & state classes in 1 of 3 ways:


    `js-` prefix for hooks

    ```html
    <button class="button js-button">Action Button</button>
    ```

    `is-` prefix for temporary state classes

    ```html
    <button class="button is-active">Active Action Button State</button>
    ```

    `--is-` modifier for temporary state classes that are element dependant

    ```html
    <button class="button button--is-active">Active Action Button State</button>
    ```

**Box Sizing**
* Skeletor's default CSS reset resets `box-sizing` to `border-box` on the `html` selector and every other element `inherits` this value.

    ```css
    html {
        box-sizing: border-box;
    }

    *, *:before, *:after {
        box-sizing: inherit;
    }
    ```

### Javascript
SFCC projects will use the JS standards (jQuery, modular JS combined into app.js) of Site Genesis.
The following standards will be applied to javascript written by UI Team members:

## Frameworks
### Skeletor

Skeletor is a [Grunt](http://gruntjs.com)-powered, [Pattern Lab](http://patternlab.io)-centric, highly-customizable web project boilerplate and build tool created by the [DEG](http://www.degdigital.com) UI team. Skeletor uses [PostCSS](http://postcss.org) for CSS processing and [JSPM](http://jspm.io)/[SystemJS](https://github.com/systemjs/systemjs) for Javascript package management, module bundling/loading, and transpilation. Full Skeletor documentation is available [here](https://github.com/degdigital/skeletor).

### Pattern Lab
Rather than being an engine to describe the whole site, SFCC UI engineers may choose to use PatternLab to build custom pages and content areas, ensuring that the markup used is in source and consistent. 
PatternLab is not used to duplicate Site Genesis, because the markup is not consistent across versions and implimentations.

## Libraries
### DEGJS

SFCC projects will not use DEGJS int he current incarnations of Site Genesis.

### jQuery

SFCC UI team members will use jQuery as long as it is a core part of Site Genesis.

## Design Patterns & Considerations

### Atomic Design
The DEG UI team encourages the use of the [Atomic Design](http://bradfrost.com/blog/post/atomic-web-design) methodology for creating design systems. The basic gist of atomic design is to break interfaces down into fundamental building blocks and work up from there. Traditional atomic design consists of 5 distinct levels:
- __Atoms__: Basic building blocks. HTML tags, such as a form label, an input or a button.
- __Molecules__: Groups of atoms bonded together. Form label, input & button combined together.
- __Organisms__: Groups of molecules joined together to form a distinct section of an interface.
- __Templates__: Groups of organisms stitched together to form pages.
- __Pages__: Specific instances of templates.

DEG uses a modified version of these levels to simplify development, tie in better with design processes & deliverables, and to use terminology that is easier for clients to understand. Our version of atomic design consists of 4 dinstinct levels:
- __Basics__: Basic building blocks. Typography, colors, & basic HTML tags.
- __Components__: Groups of basics or other components combined together. Components should be nested into parent & child relationships when applicable.
- __Templates__: Groups of components combined together to create pages.
- __Pages__: Specific instances of templates.

### Accessibility
In addition to the DEG UI Team performance specifications, the SFCC UI team will also follow these additional guidelines:
*

### Performance
In addition to the DEG UI Team performance specifications, the SFCC UI team will also follow these additional guidelines:
*

## Tooling

### IDEs/Editors
* Sublime Text
* Eclipse

### CSS
* SASS
* PostCSS if implimented

### Javascript
* None, though implimentation of an outside JS framework (such as React) may be possible. In this case, follow the recommendations of the main DEG UI document.

### Task Runners
* Gulp
* Grunt

### Visual Editors
* Photoshop
* FontAwesome

### Virtual Machines
* N/A

### ALM / Version Control
* BitBucket

### Continuous Integration
* None Yet

### Testing
* BrowserStack

### Pull Requests
* SFCC Frontend developers should do their best to follow the pull request standards set up in the main DEG UI document.
