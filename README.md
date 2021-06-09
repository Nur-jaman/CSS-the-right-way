# CSS-the-right-way
 Modular CSS Guidline  

Introduction
------------

MaintainableCSS is an approach to writing modular, scalable and maintainable CSS. Not only does this guide explain how to do this, but more importantly it explains why.

#### What does maintainable mean?

Maintainable CSS can be defined as being able to make styling changes, without worrying about accidentally causing problems elsewhere.

#### What does scalable mean?

Scalable CSS means that as CSS increases in size, it’s still easy to maintain. If you’ve ever inherited a large CSS codebase, and been afraid to make changes, you’ll sympathise with this.

#### What does modular mean?

A module is a distinct, independent unit that can be combined with other modules to form a more complex structure. In a living room, we can consider the TV, the sofa and the wall art to be modules, all coming together to create a room.

If we take one of the units away, the others still work. We don’t need the TV to be able to sit on the sofa etc. In a website the header, registration form, shopping basket, article, product list, navigation and homepage promo can all be considered to be modules.

R:01 Semantic markup and class
------------------------------

there are two type of css class. semantic and non-semantic.

#### Non-semantic

Here are some non-semantic classes:

        class="red pull-left"
        class="grid row"
        class="col-xs-4"
        

Non-semantic classes don’t convey what an element represents. At best, they give us an idea of what an element looks like. Atomic, visual, behavioural and utility classes are all forms of non-semantic classes.

#### Semantic class

Here are some semantic classes:

        class="basket"
        class="product"
        class="searchResults"
      

Semantic classes are a corner stone of MaintainableCSS. Without them, everything else makes little sense. So name something based on what it is and everything else falls into place.

R:02 Reuse
----------

in CSS **DRY** keyword is often misinterpreted as the necessity to never repeat the exact same thing twice

Striving for DRY leads to over-thought and over-engineered code. In doing so we make maintenance harder, not easier. Instead of obsessing over styles, we should focus on reusing tangible modules. Something we’ll discuss in upcoming chapters

R:03 IDs
--------

Semantically speaking, we should use an ID when there is only one instance of a thing. And we should use a class when there are several.

*   to link labels to form fields;
*   to bind internal anchors to a hash fragment in the URL; and
*   to connect up variaous ARIA attributes to help screen reader users.

Use IDs whenever you need to enable particular behaviours for browsers and assistive technology. But avoid using them for styles.

R:04 Conventions
----------------

MaintainableCSS has the following convention:

        < module>\[-<component >\]\[-<state >\] {}
          /\* Module container \*/
          .searchResults {}

          /\* Component \*/
          .searchResults-heading {}

          /\* State \*/
          .searchResults-isLoading {}

          /\* Module container \*/
          .btn {}
          
          /\* State \*/
          .btn-active{}
          .btn-success{}
      

*   component and state are both delimitted by a dash
*   names are written with lowerCamelCase
*   selectors are prefixed with the module name

R:05 Modules and component?
---------------------------

#### What Modules ?

A module is a distinct, independent unit, that can be combined with other modules to form a more complex structure.

In a living room, we can consider the TV, the sofa and the wall art modules. All coming together to create a useable room.

In a website the header, registration form, shopping basket, article, product list, navigation and homepage promo can all be considered to be modules.

#### What’s a component?

A module is made up of components. Without the components, the module is incomplete or broken.

For example a sofa is made up of the frame, upholstery, legs, cushions and back pillows, all of which are required components to allow the sofa to function as designed.

#### Modules vs components

Sometimes it’s hard to decide whether something should be a component or a module. For example, we might have a header containing a logo and a menu. Are these components or modules?

In a recent project it made most sense for the logo to be a component and the menu to be a module of its own. What’s a header without logo? And the navigation might be moved below the header.

#### Example of Module

 `<div class="basket">
  <h1 class="basket-title">Your basket</h1>
  <div class="basket-item">
    <h3 class="basket-productTitle">Product title</h3>
    <form>
      <input type="submit" class="basket-removeButton" value="Remove">
    </form>
  </div>
</div>` 
       

And CSS

    .basket {}
    .basket-title {}
    .basket-item {}
    .basket-productTitle {}
    .basket-removeButton {}
    

#### Working woth same duplication module

Next, we will build an order summary module. This module is shown during checkout and bears some resemblance to the basket. For example, it has a title and it displays a list of products.

The first thing to address is the temptation to reuse the basket template (and CSS). Even though there are similarities, this does not mean they are the same.

to avoid this problem we can create a separate modules

    <div class="basket">
      <h1 class="basket-title">Your basket</h1>
      <div class="basket-item">
        <h3 class="basket-productTitle">Product title</h3>
        <form>
          <input type="submit" class="basket-removeButton" value="Remove">
        </form>
      </div>
    </div>
    

#### Working with share module

Buttons are an example of something that we want to reuse in lot’s of places, and potentially within different modules. A button is not particularly useful on its own.

The problem is that different buttons often have slightly different positioning, sizing and spacing depending on context. And of course there is media queries to consider.

For example, within one module a button might be floated to the right next to some text. In another it might be centered with some text beneath with some bottom margin.

To avoid these problems, we can use a mixin or comma-delimit the common rules that aren’t affected by their context. For example:

    .basket-removeButton,
    .another-loginButton,
    .another-deleteButton {
      background-color: green;
      padding: 10px;
      color: #fff;
    }
  

R:06 State
----------

Quite often, particularly with richer user interfaces, styling needs to be applied in response to an element’s change of state. For example, we may have different styles when a module (or component) is:

*   showing or hiding;
*   active or inactive;
*   disabled or enabled;
*   loading or loaded;
*   hasProducts or hasNoProducts;
*   isEmpty or isFull;

To represent state we need an additional class which should be added to the module (or component) element to which it pertains. For example, if our basket module needs a gray background when it’s empty, the HTML should be:

    <div class="basket basket-isEmpty">
    

If an element’s style needs changing based on its state, we should add an extra class to apply the differences. When necessary, use ARIA attributes for assistive technologyy, not for styling. In doing so we employ a consistent and inclusive approach to styling.

R:07 Modifiers
--------------

Like state, a modifier can also override styles. This is useful when modules (or components) have small and well understood differences.

Take an e-commerce site whereby each category has a unique background image in the header.

All headers have the same padding, and margin etc. The only difference is the background image. For example, the boys category would have a modifier as follows:

    `<div class="categoryHeader categoryHeader-boys">` 
  

R:08 Versioning
---------------

We may, for example, want to A/B test two different versions of a module to see which works best. To do this, we need to duplicate the module and give it a unique name. For example, if we want to test two different baskets, the CSS might be as follows:

  /\* existing module (variant A) \*/
  .basket {}

  .basket-title {}

  /\* new version (variant B) \*/
  .basket2 {}

  .basket2-title {}
  

R:09 Structure Guideline
------------------------

#### Syntax and Formatting

*   two (2) space indents, no tabs;
*   80 character wide columns;
*   multi-line CSS;
*   meaningful use of whitespace.

#### Table of Contents

A simple table of contents will—in order, naturally—simply provide the name of the section and a brief summary of what it is and does, for example:

  /\*\*
   \* CONTENTS
   \*
   \* SETTINGS
   \* Global...............Globally-available variables and config.
   \*
   \* TOOLS
   \* Mixins...............Useful mixins.
   \*
   \* GENERIC
   \* Normalize.css........A level playing field.
   \* Box-sizing...........Better default \`box-sizing\`.
   \*
   \* BASE
   \* Headings.............H1–H6 styles.
   \*
   \* OBJECTS
   \* Wrappers.............Wrapping and constraining elements.
   \*
   \* COMPONENTS
   \* Page-head............The main page header.
   \* Page-foot............The main page footer.
   \* Buttons..............Button elements.
   \*
   \* TRUMPS
   \* Text.................Text helpers.
   \*/
  

#### 80 Characters Wide

Where possible, limit CSS files’ width to 80 characters. Reasons for this include

*   the ability to have multiple files open side by side;
*   viewing CSS on sites like GitHub, or in terminal windows;
*   providing a comfortable line length for comments.

#### Titling

Begin every new major section of a CSS project with a title:

      /\*------------------------------------\*\\
      #SECTION-TITLE
      \\\*------------------------------------\*/

      .selector { }
    

The title of the section is prefixed with a hash (#) symbol to allow us to perform more targeted searches (e.g. grep, etc.): instead of searching for just SECTION-TITLE—which may yield many results—a more scoped search of #SECTION-TITLE should return only the section in question.

If you are working on a project where each section is its own file, this title should appear at the top of each one. If you are working on a project with multiple sections per file, each title should be preceded by five (5) carriage returns. This extra whitespace coupled with a title makes new sections much easier to spot when scrolling through large files:

      /\*------------------------------------\*\\
      #A-SECTION
      \\\*------------------------------------\*/

      .selector { }





      /\*------------------------------------\*\\
      #ANOTHER-SECTION
      \\\*------------------------------------\*/

      /\*\*
      \* Comment
      \*/

      .another-selector { }
    

#### Anatomy of a Ruleset

      \[selector\] {
        \[property\]: \[value\];
        \[ <--declaration--->\]
      }
    

#### Multi-line CSS

*   A reduced chance of merge conflicts, because each piece of functionality exists on its own line.
*   More ‘truthful’ and reliable diffs, because one line only ever carries one change.

  .icon--home     { background-position:   0     0  ; }
  .icon--person   { background-position: -16px   0  ; }
  .icon--files    { background-position:   0   -16px; }
  .icon--settings { background-position: -16px -16px; }
  

#### Indenting

This quasi-replication of the DOM tells developers a lot about where classes are expected to be used without them having to refer to a snippet of HTML.

    .foo { }

      .foo\_\_bar { }

        .foo\_\_baz { }
  

#### Alignment

Attempt to align common and related identical strings in declarations, for example:

    .foo {
      -webkit-border-radius: 3px;
         -moz-border-radius: 3px;
              border-radius: 3px;
    }

    .bar {
      position: absolute;
      top:    0;
      right:  0;
      bottom: 0;
      left:   0;
      margin-right: -10px;
      margin-left:  -10px;
      padding-right: 10px;
      padding-left:  10px;
    }
  

#### Preprocessor Comments

With most—if not all—preprocessors, we have the option to write comments that will not get compiled out into our resulting CSS file. As a rule, use these comments to document code that would not get written out to that CSS file either. If you are documenting code which will get compiled, use comments that will compile also. For example, this is correct:

// Dimensions of the @2x image sprite:
$sprite-width:  920px;

/\*\*
 \* 1. Default icon size is 16px.
 \* 2. Squash down the retina sprite to display at the correct size.
 \*/
    

#### Hyphen Delimited

Camel case and underscores are not used for regular standard classes; the following are incorrect:

#### JavaScript Hooks

As a rule, it is unwise to bind your CSS and your JS onto the same class in your HTML. This is because doing so means you can’t have (or remove) one without (removing) the other. It is much cleaner, much more transparent, and much more maintainable to bind your JS onto specific classes.

#### CSS Selectors

Perhaps somewhat surprisingly, one of the most fundamental, critical aspects of writing maintainable and scalable CSS is selectors. Their specificity, their portability, and their reusability all have a direct impact on the mileage we will get out of our CSS, and the headaches it might bring us.

Poor Selector Intent is one of the biggest reasons for headaches on CSS projects. Writing rules that are far too greedy—and that apply very specific treatments via very far reaching selectors—causes unexpected side effects and leads to very tangled stylesheets, with selectors overstepping their intentions and impacting and interfering with otherwise unrelated rulesets.

**your selectors should be as explicit and well reasoned as your reason for wanting to select something.**

#### Location Independence

our components’ styling should not be reliant upon where we place them—they should remain entirely location independent.

Let’s take an example of a call-to-action button that we have chosen to style via the following selector:

      .promo a { }
    

Not only does this have poor Selector Intent—it will greedily style any and every link inside of a .promo to look like a button—it is also pretty wasteful as a result of being so locationally dependent: we can’t reuse that button with its correct styling outside of .promo because it is explicitly tied to that location. A far better selector would have been: **A component shouldn’t have to live in a certain place to look a certain way.**

Better One

    .btn { }
    

#### Naming

Phil Karlton once said, _There are only two hard things in Computer Science: cache invalidation and naming things._

Tying your class name semantics tightly to the nature of the content has already reduced the ability of your architecture to scale or be easily put to use by other developers. _Using a class name to describe content is redundant because content describes itself._

It is important to strike a balance between names that do not literally describe the style that the class brings, but also ones that do not explicitly describe specific use cases. Instead of

  .home-page-panel use  .masthead;
  .site-nav, use .primary-nav;
  .btn-login, use .btn-primary.
       

Selector Performance
--------------------

A topic which is—with the quality of today’s browsers—more interesting than it is important, is selector performance. That is to say, how quickly a browser can match the selectors your write in CSS up with the nodes it finds in the DOM.

Generally speaking, the longer a selector is (i.e. the more component parts) the slower it is, for example:

**Very Bad**

         body.home div.header ul { }
       

Good One

         .primary-nav { }
       

This is because browsers read CSS selectors right-to-left. A browser will read the first selector as

*   find all `ul` elements in the DOM;
*   now check if they live anywhere inside an element with a class of `.header`;
*   next check that `.header` class exists on a `div` element;
*   now check that that all lives anywhere inside any elements with a class of `.home`;
*   finally, check that `.home` exists on a `body` element.

#### Effecient child selector

By using a child selector (e.g. .foo > .bar {}) we can make the process much more efficient, because this only requires the browser to look one level higher in the DOM, and it will stop regardless of whether or not it found a match.

*   **Select what you want explicitly**, rather than relying on circumstance or coincidence. Good Selector Intent will rein in the reach and leak of your styles.
*   **Write selectors for reusability**, so that you can work more efficiently and reduce waste and repetition.
*   **Do not nest selectors unnecessarily**, because this will increase specificity and affect where else you can use your styles.
*   **Do not qualify selectors unnecessarily**, as this will impact the number of different elements you can apply styles to.
*   **Keep selectors as short as possible**, in order to keep specificity down and performance up.

#### Nesting

We’ve already looked at how nesting can lead to location dependent and potentially inefficient code, but now it’s time to take a look at another of its pitfalls: it makes selectors more specific.

Whether you arrive at this CSS via a preprocessor or not isn’t particularly important, but it is worth noting that preprocessors tout this as a feature, where is actually to be avoided wherever possible.

**if a selector will work without it being nested then do not nest it.**
