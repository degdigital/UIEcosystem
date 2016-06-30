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


## Code Style Guide

### HTML
* Semantic Markup

### CSS
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
* Selectors should use a Pseudo BEM methodology (See BEM below)
* Use ID selectors sparingly, if at all
* When using multiple selectors in a rule declaration, give each selector its own line.
* Properties should be organized in the most logical order
* When defining multiple properties, give each property its own line.

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
* DEG's approach is to follow the general guidelines of BEM and it's syntax, though it is not strictly enforced, which is why it's referred to as Pseudo BEM here. For full documentation on the BEM methodology, reference out the [BEM website](http://getbem.com/).

**Variables**

*[variable naming structure/syntax]

**Nested Selectors**
* Following BEM or pseudo BEM should allow you to avoid nesting in most cases. This creates CSS that is both easier to maintain and smaller in size. When nesting does become needed, it should be kept as shallow as possible. A good code smell is to refactor and break out CSS that requires nesting beyond three levels deep.

**Breakpoints & Media Queries**
* __Write Mobile First CSS.__ Authoring mobile-first styles results in smaller, simpler, more maintainable code and is in line with DEG's stance on progressive enhancement.

    __Bad__

    ```css
    .column {
        float: left;
        width: 50%;
    }
    @media all and (max-width: 50em) {
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
* __Don’t use device dimensions to determine breakpoints.__ The device landscape is always changing, so today’s values might be moot even just a year down the road. The Web is inherently fluid, so it’s our job to create interfaces that look and function beautifully on any screen instead of in just a few arbitrary buckets.

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

### Javascript
**ES6**
* Template Strings

## Frameworks
### Skeletor

Skeletor is a [Grunt](http://gruntjs.com)-powered, [Pattern Lab](http://patternlab.io)-centric, highly-customizable web project boilerplate and build tool created by the [DEG](http://www.degdigital.com) UI team. Skeletor uses [PostCSS](http://postcss.org) for CSS processing and [JSPM](http://jspm.io)/[SystemJS](https://github.com/systemjs/systemjs) for Javascript package management, module bundling/loading, and transpilation. Full Skeletor documentation is available [here](https://github.com/degdigital/skeletor).

### Pattern Lab
Pattern Lab is a collection of tools to help you create atomic design systems. DEG uses Pattern Lab as a design & development tool, a prototyping & demo tool, and as an interactive style guide deliverable for clients. Although Pattern Lab comes with an out of the box partner starter kit, we have modified this kit to more closely resemble the types of projects we work on and practices we follow. Our modified version of Pattern Lab can be found within [Skeletor](https://github.com/degdigital/skeletor). Pattern Lab specific documentation can be found on the [Pattern Lab website](http://patternlab.io/).

## Libraries
### DEGJS

DEGJS is a curated list of ES6-formatted JavaScript modules, encompassing front-end functionality and utilities, developed by the DEG UI team. All modules are hosted under our [DEGJS GitHub account](https://github.com/degjs), and are formatted to work with Babel, the JSPM package manager and its accompanying JavaScript loader, System.js.

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
[Accessibilty guidelines here]
* Alt Tags
* Aria Roles

### Performance
Because speed and performance is a vital part of any website, DEG encourages the use of performance budgets to help guide design and development decisions.

A performance budget is like any other project budget: it defines a clear goal that the project can be evaluated against to determine success. More specifically, a performance budget sets limits for individual pages of a website that should not be exceeded. These limits can be expressed in the form of several metrics, such as how long it takes for a page to load and how many kilobytes a page weighs.
A performance budget helps you choose how to display content or define functionality. It does not dictate what content should be displayed. Removing something important to decrease page weight is not a good performance strategy.
The overall benefit of a performance budget is a fast, lightweight website.

For more details on how and when DEG sets performance budgets, view our [Performance Budget Guide](https://docs.google.com/document/d/1JobZThkKpRAtSGHGZ7aCJ5Ub_1q2SJqohDPjQmjeLiE/edit) on Google Docs.

### Detailed Design Patterns & Anti-Patterns
Although projects do often present unique challenges, there are certain challenges we see repeated across many projects. Because of this, the DEG UI team maintains a set of client facing documentation on the best practices for using and implementing common design patterns & anti-patterns including:
- [Carousels](https://docs.google.com/document/d/1iBiWISTsRwTv-Jc7VEJGHhVXnru8Tyf2YZ4olHvjg-g/edit)
- [Hover Interactions](https://docs.google.com/document/d/1L_ppJEh24ly_R_LgC_9whqylc9FnkAokNAEJFuDLbY8/edit)
- [Infinite Scrolling vs Pagination](https://docs.google.com/document/d/1C4XFaIfv2Pt0dyRjGYZVUTo5sTw-RzqYk8ingLuxfOI/edit)
- [Mega Menus](https://docs.google.com/document/d/1l7t2MpYf1Ux-RtRyzD2NKWinuLwmD4IYFX8emtpJFcc/edit)
- [The Fold](https://docs.google.com/presentation/d/1baCcqMkYE0h3S7oP637ZMjK106bUpyHNRMw2NW7a4BE/edit?usp=drive_web)
- [Web Fonts](https://docs.google.com/document/d/16_8FOMAAdCp2FL-vtSkj72uUVYpiBaYRUw0JrlAdpQE/edit)

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
