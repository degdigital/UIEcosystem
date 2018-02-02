# UI Ecosystem
*DEG UI Code Standards, Design Patterns, Frameworks & Tooling*

## Table of Contents
  1. [Code Style Guide](#code-style-guide)
    - [HTML](#html)
    - [CSS](#css)
    - [Javascript](#javascript)
  2. [Design Patterns & Considerations](#design-patterns--considerations)
    - [Atomic Design](#atomic-design)
    - [Browser Support](#browser-support)
    - [Accessibility](#accessibility)
    - [Performance](#performance)
    - [Detailed Design Patterns & Anti-Patterns](#detailed-design-patterns--anti-patterns)
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
    - [Virtual Machines](#virtual-machines)
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
While DEG doesn't maintain a specific code style guide in relation to Javascript, we do utilize es6 features and syntax and tend to follow the guidelines set out by airbnb in their [Javascript style guide](https://github.com/airbnb/javascript).

**JSPM/SystemJS**
* DEG utilizes [JSPM](http://jspm.io/) and [SystemJS](https://github.com/systemjs/systemjs) for Javascript package management, module bundling/loading, and transpilation. For more detailed information on how these tools are used, refer to the [Javscript section](https://github.com/degdigital/skeletor#javascript) of the Skeletor documentation.

**Modules**
* DEG utilizes Javascript modules to create a maintainable, reusable, and performant codebase rather than a sprawling and interdependent one.
* Small, self-contained modules with distinct functionality are preferred over large, all inclusive modules. This allows for modules that can be [shuffled, removed, or added as necessary](https://medium.freecodecamp.com/javascript-modules-a-beginner-s-guide-783f7d7a5fcc#.xi6wgrvv2), without disrupting the system as a whole.

**Polyfilled Bundles**
* In many cases, Javascript code will require certain feature polyfills in order to run correctly on legacy (or even modern) browsers. For delivering these polyfills to the client, DEG takes the [conditional-build approach](https://github.com/SlexAxton/yepnope.js#deprecation-notice) recommended by the yepnope.js team and others. For more details on how to generate conditional builds, refer to the [Polyfilled Bundles](https://github.com/degdigital/skeletor#polyfilled-bundles) section of the Skeletor documentation.

## Frameworks
### Skeletor

Skeletor is a [Grunt](http://gruntjs.com)-powered, [Pattern Lab](http://patternlab.io)-centric, highly-customizable web project boilerplate and build tool created by the [DEG](http://www.degdigital.com) UI team. Skeletor uses [PostCSS](http://postcss.org) for CSS processing and [JSPM](http://jspm.io)/[SystemJS](https://github.com/systemjs/systemjs) for Javascript package management, module bundling/loading, and transpilation. Full Skeletor documentation is available [here](https://github.com/degdigital/skeletor).

### Pattern Lab
Pattern Lab is a collection of tools to help you create atomic design systems. DEG uses Pattern Lab as a design & development tool, a prototyping & demo tool, and as an interactive style guide deliverable for clients. Although Pattern Lab comes with an out of the box pattern starter kit, we have modified this kit to more closely resemble the types of projects we work on and practices we follow. Our modified version of Pattern Lab can be found within [Skeletor](https://github.com/degdigital/skeletor). Pattern Lab specific documentation can be found on the [Pattern Lab website](http://patternlab.io/).

## Libraries
### DEGJS

[DEGJS](https://github.com/degjs) is a curated list of ES6-formatted JavaScript modules, encompassing front-end functionality and utilities, developed by the DEG UI team. All modules are hosted under our [DEGJS GitHub account](https://github.com/degjs), and are formatted to work with Babel, the JSPM package manager and its accompanying JavaScript loader, System.js.

### jQuery

__* Magento & Legacy Only *__ The ubiquitous javascript library. Many legacy projects as well as anything on the Magento platform will include the jQuery library. The DEG UI team recommends against using jQuery when possible.

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

### Browser Support
DEG uses the concept of Graded Browser Support, which defines the set of browsers that should receive a verified, usable experience. However, trying to deliver the same "A-grade" experience across all tested browsers is neither cost-effective nor common. We support a tiered approach to user experience design, development, and testing, and encourage each project to define their own tiers that serve their users and stakeholders best. For a more detailed explanation and a guide to help determine what browsers to support, view our [Browser Support Guide](https://docs.google.com/document/d/1RDcfLoOyj-zwz7JmFxV6KQVlZ_yuZhhSMornd-uikaE/edit#) on Google Docs.

### Accessibility
DEG Standards of Quality stipulate a minimum of WCAG 2.0 Level A accessibility conformance. However, some projects may have more strict accessibility needs. For WCAG 2.0 Level A development requirements, reference the [WCAG 2.0 Checklist](http://webaim.org/standards/wcag/checklist).

### Performance
Because speed and performance is a vital part of any website, DEG encourages the use of performance budgets to help guide design and development decisions.

A performance budget is like any other project budget: it defines a clear goal that the project can be evaluated against to determine success. More specifically, a performance budget sets limits for individual pages of a website that should not be exceeded. These limits can be expressed in the form of several metrics, such as how long it takes for a page to load and how many kilobytes a page weighs.
A performance budget helps you choose how to display content or define functionality. It does not dictate what content should be displayed. Removing something important to decrease page weight is not a good performance strategy.
The overall benefit of a performance budget is a fast, lightweight website.

For more details on how and when DEG sets performance budgets, view our [Performance Budget Guide](https://docs.google.com/document/d/1JobZThkKpRAtSGHGZ7aCJ5Ub_1q2SJqohDPjQmjeLiE/edit) on Google Docs.

### Detailed Design Patterns & Anti-Patterns
Although projects do often present unique challenges, there are certain challenges we see repeated across many projects. Because of this, the DEG UI team maintains a set of client facing documentation on the best practices for using and implementing common design patterns & anti-patterns including:
- [Carousels](https://docs.google.com/document/d/1iBiWISTsRwTv-Jc7VEJGHhVXnru8Tyf2YZ4olHvjg-g/edit)
- [Designing For Touch](https://docs.google.com/document/d/1xncXc-5xsy9DWM0NXOBsHlwndP-51Ff_WiG72VXxv2s/)
- [Hover Interactions](https://docs.google.com/document/d/1L_ppJEh24ly_R_LgC_9whqylc9FnkAokNAEJFuDLbY8/edit)
- [Infinite Scrolling vs Pagination](https://docs.google.com/document/d/1C4XFaIfv2Pt0dyRjGYZVUTo5sTw-RzqYk8ingLuxfOI/edit)
- Mega Menus [In Progress]
- [The Fold](https://docs.google.com/presentation/d/1baCcqMkYE0h3S7oP637ZMjK106bUpyHNRMw2NW7a4BE/edit?usp=drive_web)
- [Web Fonts](https://docs.google.com/document/d/16_8FOMAAdCp2FL-vtSkj72uUVYpiBaYRUw0JrlAdpQE/edit)
- [Forms Don't Have To Suck](https://docs.google.com/presentation/d/1SLLszWMKQjiKtF7odlrWGbUy7hl6l5C4VHK923w_zQE/edit)

## Tooling

### IDEs/Editors
* Sublime Text
* PHPStorm
* Visual Studio

### CSS
* PostCSS
* Legacy Projects: Sass/Compass

### Javascript
* JSPM - Package Management
* SystemJS - Module Bundling

### Task Runners
* Grunt

### Visual Editors
* Photoshop
* Illustrator
* Sketch
* IcoMoon

### Virtual Machines
* VirtualBox
* Vagrant

### ALM / Version Control
* JIRA
* Assembla
* Git

### Continuous Integration
* Jenkins

### Testing
* BrowserStack

### Pull Requests
Pull Requests are both a great way to maintain high quality code and an opportunity to learn from each other. Here are a few best practices to get you started:

**Submitting a Pull Request**

* It's much better to submit small pull requests often. Shoot to submit at least one Pull Request (PR) per day. If your ticket is bigger than that. Try to break it into smaller chunks. It's much easier for a reviewer to read through a four file change than it is to read through a massive refactor.

**Reviewing a Pull Request**

* Who can review? Simple, everyone should review code. If you are new to a project, you'll get a feeling for the structure other projects. If you are experienced, you'll be able to offer more insights.

* Each project will need to decide who is able to officially approve a request. If you are principle reviewer, __try to finish a review within 24 hours__. When you approve a PR, leave a comment saying it looks good or a simple :+1:. Some systems have official approval buttons.

* Merging a pull request is the responsibility of the person who opened the PR. Why? There may be additional tasks such as deploying or updating a tag. 

* When reviewing, open the code in a browser if possible. It can be hard to grasp a change until you see it live.

* Remember to be polite. As [one guideline says](https://github.com/thoughtbot/guides/tree/master/code-review) "Accept that many programming decisions are opinions. Discuss tradeoffs, which you prefer, and reach a resolution quickly."

* We are all learning. As long as the code does not violate agreed standards, the author has a right to use whatever solution they want.

* For more guidelines on review code, see the [thoughtbot guide](https://github.com/thoughtbot/guides/tree/master/code-review#everyone) and the guide by [github](https://github.com/blog/1943-how-to-write-the-perfect-pull-request).

**What To Look for When Reviewing Code**

* There are some things that must be completed before code can be accepted. These include following linting rules and testing requirements if applicable.

* Syntax suggestions:
```javascript
const settings = Object.assign({}, defaults, options);
```
  A review might say:
  > Consider using object spread:
```javascript
const settings = {
  ...defaults,
  ...options
}
```

* Code Reuse
```javascript
function getNames(people) {
  return people.map(person => person.name);
}

function getAges(people) {
  return people.map(person => person.age);
}
```
Reviewer:
> You should consider a reusable function
```javascript
function extractField(group, field) {
  return group.map(item => item[field]);
}

getNames(people) {
 return extractField(people, 'names')
}
```

* Readability
```javascript
function createSidebar(data) {
 let small = true;
 let sidebar;
 if(data.length > 20) {
   small = false;
 }
 // 50 more lines of data manipulation
```
 Reviewer:
 > I get a little lost on this function. Could you split it up?
 
 * Incorporating libraries
 ```javascript
 // File someproject/bankingForm.js
 function checkPassword(element, rules) {
 // check password for strength
 }
 ```
 Reviewer:
 > We have a library called [Password Strength](https://github.com/DEGJS/passwordStrength) you might check to see if that would work in this situation.
 
 * Catching potential bugs
 ```javascript
 function getTotalMileage(reports) {
   reports.reduce((total, report) => {
     return total + report.miles
    }, 0)
  }
  ```
  Reviewer:
  > Looks like you forgot to return the total after running the reduce function. That'll return `undefined` everytime.
  
  * And more 
  
  There are many more things you may notice. It's ok to ask just remember to be polite.
  ```
    
