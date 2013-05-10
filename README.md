# SASS conventions (draft)

This document outlines the conventions we have at Le Wattman for writing SASS code.
It should be strictly applied, and any exception to the rule should be discuseed first.
It's a living document, updated if a consensus applies.


Anyone is very welcomed to comment on this.


## Table of content

- Inspired from
- Goals
- Idiomatic CSS
- Object oriented SASS
- Frameworks, libraries
- Production code
- File structure
  - Rules
  - Global variables
  - Inline stylesheet
  - IE stylesheet
  - Main stylesheet
- Naming convention
  - Modules
  - Layout
  - States
  - Colors
- Properties
  - Ordering
- Functions / Mixins
- Testing
- To do


## Inspired from

We don't invent anything here, it's just a aggregation of things found in the
following list we like :

- https://github.com/necolas/idiomatic-css
- https://github.com/anthonyshort/idiomatic-sass
- https://github.com/sturobson/a-slightly-bizarre-starting-point
- http://smacss.com


## Goals

- It's not to define the best convention ever, but to share one and stick to it
- Reuse a maximum of things across projects
- 

## Idiomatic CSS

Largely taken from https://github.com/necolas/idiomatic-css

`Line length :`
- 80 columns

`Whitespaces :`
- configure your editor to "show invisibles" or to automatically remove end-of-line whitespace.

`Comments :`
- Always use ```//``` SASS comments, they will not be outputed. Use regular comment if you have a good reason

`Format :`
- Use one discrete selector per line in multi-selector rulesets.
- Include one declaration per line in a declaration block.
- Include a single space after the colon of a declaration.
- Use lowercase and shorthand hex values, e.g., #aaa.
- Use (single or) double quotes consistently. Preference is for double quotes, e.g., content: "".
- Quote attribute values in selectors, e.g., input[type="checkbox"].
- Where allowed, avoid specifying units for zero-values, e.g., margin: 0.
- Include a space after each comma in comma-separated property or function values.
- Include a semi-colon at the end of the last declaration in a declaration block.
- Place the closing brace of a ruleset in the same column as the first character of the ruleset.
- Separate each ruleset by a blank line.

## Misc rules

- apply every state of a module in the module's file (media queries, pseudo elements, js triggered states)

  .module
    blah blah
    
    &:hover
      pseudo element state

  .is-triggered-by-js
    state related to .module only

- 2 nesting levels max
- no IDs
- indentation: 2 spaces
- 

## Object oriented SASS

- decoupled : a module should never manipulate another module
- never use IDs 
  It adds to much specificity. Actually, this may sound stupid but we prefer 
  break a too strict rule than being no strict enough

## Frameworks, libraries

- Compass
- Susy for grids
- worth checking : sassifaction
- to test : https://github.com/aaronjensen/sprockets-media_query_combiner

If working outside a Rails app, you may want use : 

- https://github.com/chriseppstein/sass-globbing

## Production code

- sass output : compressed
- inlined style : ..

## File structure

Follow the structure given by SMACSS, plus some things : 

  +- stylesheets
    +- base
      +- baseline.sass
      +- colors.sass
      +- grid.sass
      +- index.sass // includes all /base styles in specific order
      +- typography.sass
    +- layout
    +- module
    +- state
    +- utilities
      +- "lib" folder, for custom mixins
    inline.sass
    screen.sass
    ie.sass

- ```inline.sass``` is for the basic styles which may be included in the <head> of a layout.
- ```screen.sass``` refers to the main stylesheet
- ```ie.sass``` refers to the stylesheets 

### Rules (taken from https://github.com/anthonyshort/idiomatic-sass#file-structure)

- Each logical module of code should belong in its own file. Avoiding putting multiple objects in the same file. This allows you to use the filesystem to navigate your Sass rather than relying on comment blocks.
- Mixins/placeholders/functions should, if possible, belong in their own file.
- Files should be named for the component they are housing. A block-list object will live in a block-list.scss file.

### Global variables

We don't have a ```globals.sass``` file, instead we 

- ```$inlined``` : used to conditionnaly load some code in /base mainly
- ```$legacy-support-for-ie6``` : Compass var
- ```$legacy-support-for-ie7``` : Compass var
- ```$legacy-support-for-ie8``` : Compass var

### Inline stylesheet

The aim is to inline in <head> some very basic styles (like header, footer, typography)
in order to gain some perceived performance.

  $inlined: true
  $legacy-support-for-ie6: false
  $legacy-support-for-ie7: false
  $legacy-support-for-ie8: false

  @import "utilities/*"
  @import "compass/reset"
  @import "base/index"

Compass reset should be logically imported from base/index, but since base/index is 
also imported by ```screen.sass``` and since we can't import conditionnaly a file in SASS, 
we just add here.

### IE stylesheet

The usual case is a mobile first approach : without any media querie, we have a mobile layout.
For IE's version which don't support MQ, there is 2 solutions : 
- respond.js but it adds an overhead https://github.com/scottjehl/Respond
- deliver a 

`Goal` : 
1. an IE stylesheet generated automatically
2. all MQ styles without MQ


  .lt-ie8 & {

    // put the IE8 and below CSS declarations here
    /* IE8 and below */

    // if you need IE7 as well then prefix the CSS with a *
    /* IE7 and below */

    // if you need IE6 as well then prefix the CSS with a /
    /* IE6 */
    }
  }


### Main stylesheet


  $inlined: false
  $legacy-support-for-ie6: false
  $legacy-support-for-ie7: false
  $legacy-support-for-ie8: false

  @import "utilities/*"
  @import "base/index"
  @import "layout/*"
  @import "module/*"
  @import "state/*"


## Naming convention

- .only-hyphens-really
- module/layout/state/utility name = filename

### Modules

No prefix

### Layout
  
  .l-example

### States
  .is-example

### Colors

// ---- literal colors ----
$black: black
$grey: #ccc
...

// ---- semantic color mappings ----
$primary-color: $cyan
$accent-color: $light-blue
$alert-color: $red

// text
$light-text-color: $white
$medium-text-color: $grey
$dark-text-color: $dark-blue


## Properties

### Ordering (taken from https://github.com/anthonyshort/idiomatic-sass#ordering)

0. variables
1. @extend directives
2. @include directives
3. the rest


A strictier ruleset could be applied, like for example alphabetical order, but
we will try to stick to this basic ruleset for now.

todo: example

## Functions / Mixins

- Must be located in /utilities
- Must be as generic as possible
- When tied to a module, the name of 
- If project specific utility, prefix with ```name-of-project-```
- If a utility may be reused in another project, prefix with ```wtm-```

Use defaults var values when possible ? 
- document 

### Functions

todo

### Mixins (taken from https://github.com/anthonyshort/idiomatic-sass#mixins)

Mixins should only be used when there are dynamic properties, otherwise use @extend
Mixins that output selectors should be capital-case: @mixin GridBuilder
Mixins that output only properties should be camel-case: @mixin borderBox
Mixins should be prefixed if they are part of a public module: @mixin as-GridBuilder
In general, mixins with logic should not be longer than ~50 lines just like any other programming language
Private mixins that are not used outside of the current file should be prefixed with a dash: @mixin -gridHelper
Avoid using more than 4 parameters. It is a sign that a mixin is too complex. When Sass adds hashes life will be easier.
Mixins should be documented

## Testing

todo : http://compass-style.org/help/tutorials/testing/


## To do

- include basic file structure example in this repo
- test Bower package management workflow
- https://github.com/styleguide/css
- https://github.com/kneath/kss
- test and document sass-import-once
