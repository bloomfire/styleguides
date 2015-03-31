Bloomfire Code Style Guide: CSS
===============================

Introduction
------------

It’s easy to write CSS. It’s hard to write CSS that’s clear, maintainable, and extensible. These guidelines aim to help developers of all skill levels and degrees of familiarity with the codebase write clean, sensible CSS. The goal is to have code that looks like a single person wrote it, no matter how many people have had their hands in it. Not only does this give us code that’s easier to read, but it also speeds up development by providing a consistent, familiar environment to code in.

This guide is necessarily opinionated, but not arbitrarily so &ndash; it incorporates generally-accepted best practices as well as hard-won lessons. (See [Credits and Additional Reading](#credits-and-additional-reading) for more.) It also aims to be fairly comprehensive; but if you find yourself up against something that’s not covered here, observe the existing style as closely as possible.


Table of Contents
-----------------

* [Syntax and Formatting](#syntax-and-formatting)
  - [Whitespace](#whitespace)
  - [Declaration Order](#declaration-order)
  - [Shorthand](#shorthand)
  - [Zeroes](#zeroes)
  - [Quotes](#quotes)
* [Archtecture](#architecture)
  - [File Naming Conventions](#file-naming-conventions)
  - [File Size](#file-size)
* [CSS Naming Conventions](#css-naming-conventions)
* [Comments](#comments)
* [SCSS](#scss)
  - [Nesting](#nesting)
  - [Variables](#variables)
  - [`@extend`s and Placeholders](#extends-and-placeholders)
* [Credits and Additional Reading](#credits-and-additional-reading)


Syntax and Formatting
---------------------

First, a little sketch to make sure we’re on the same page as far as terminology is concerned:

```
// Anatomy of a CSS ruleset
selector {
  property: value;
}
```

The `property: value` pair is a ‘rule’ or a ‘declaration’.

###Whitespace

CSS doesn’t care about whitespace all that much, but following these rules will not only make the code easier to read, but also help out in line-by-line cut-and-paste operations.

* Use spaces, not tabs. Use two spaces, not four. Spaces keep the code looking the same across platforms &ndash; GitHub handles tabs particularly poorly.
* Put each selector on its own line.
* Put each declaration on its own line.
* Put one space between a selector and the opening brace.
* Put one space between a property and its value.
* End every declaration with a semicolon.
* The closing brace gets its own line.
* Separate rulesets by one blank line.

```scss
// Nope

.foo, .bar, .baz
{ 
  display:block;
  background-color:$bg-color }
.qux { margin: 10px }

// Yep

.foo,
.bar,
.baz {
  background-color: $bg-color;
  display: block;
}

.qux {
  margin: 10px;
}
```

###Declaration Order

**Alphabetize your declarations.** Some developers don’t bother with an ordering strategy and just let the declarations fall where they may. Let us speak no further of these people.

Ordering strategies are usually based on grouping declarations by position, margin/padding, color, etc. There’s nothing wrong with that, but trying to remember a grouping order requires extra mental overhead for little to no benefit. There’s only one alphabetizing strategy, and you probably already have a pretty good handle on it; so please use it.

One exception: in `margin` and `padding` rules, order them `top`, `bottom`, `left`, and `right` to keep things tidy.

```scss
// You'd normally just want to use shorthand for the margin here, but:
.foo {
  background-color: $bg-color;
  margin-top: 10px;
  margin-bottom: 20px;
  margin-left: 5px;
  margin-right: 10px;
  padding: 10px;
}
```

Place mixin and `@extend` calls before your style declarations.

###Shorthand

**Only use shorthand declarations when you need to declare all the properties on an element.** They’re handy, but can be dangerous if used indiscriminately. Be surgical and only change the values you need &ndash; it’s a little more verbose, but it can save you potential future mental anguish.

```scss
.button {
  margin: 10px auto;
  padding: 10px 30px;
}

// Nope
.sidebar .button {
  margin: 5px auto 10px;
  padding: 10px;
}

// Yep
.sidebar .button {
  margin-top: 5px;
  margin-bottom: 10px;
  padding-left: 10px;
  padding-right: 10px;
}
```

*Exception:* If you’re clearing all the margin or padding from an element, feel free to use `margin: 0;` or `padding: 0;`.

###Zeroes

* Always use unitless `0`, e.g. `margin: 0;` instead of `margin: 0px;`.
* Include the `0` before a decimal number for easy identification as a decimal (e.g. `font-size: 0.8px;`, `rgba(0, 0, 0, 0.5)`).

###Quotes

**Single-quote strings.** CSS plays okay with double quotes, and even no quotes if the string doesn’t contain any spaces; but use single quotes for consistency.

```scss
.foo {
  background-image: url('http://placecage.com/300/300');
  font-family: 'Comic Sans', 'Arial', sans-serif;
}
```

(`sans-serif` isn’t quoted here because it’s a [CSS value](https://developer.mozilla.org/en-US/docs/Web/CSS/font-family), not a string.)


Architecture
------------

For current ongoing projects, we maintain three main CSS files, all of which live in `/assets/stylesheets/v3/`. `main.scss` is our main CSS file which imports all of our modules and helpers. Due to IE’s selector limit, though, we’ve had to split `main.scss` into `main1.scss` and `main2.scss`. These files should stay in sync, with the `@import`s that aren’t used in that particular main CSS file commented out. Currently, all new CSS is being added to `main2.scss`.

*tl;dr:* When you add a new partial, add it to all three files and comment it out in `main1.scss`.

###File Naming Conventions

Begin a new partial name with an underscore, separating words with hyphens; and use the extension `.scss`.

```scss
// Nope
myAwesomeNewPartial.css.scss

// Yep
_my-awesome-new-partial.scss
```

###File Size

We use the Sass `@import` directive to concatenate our CSS into one file (well, two files, actually; see above), so creating new CSS partials is fairly cheap &ndash; no need to worry about excessive HTTP requests like you do with the vanilla CSS `@import`. There’s necessarily no hard-and-fast rule about the maximum length of a partial; but if you find yourself scrolling around a lot trying to find something or start losing your place while jumping around in a file, it may be worth considering breaking out some of the rules into a new partial. I usually find this happens at around 200 to 300 lines.


CSS Naming Conventions
----------------------

* Never use IDs in CSS! They can be a quick fix, but it makes it very easy to run into specificity problems, which you then have to paper over with `!important`, which is just asking for trouble.
* Avoid overqualifying your selectors (e.g. `div.foo`, `ul#bar`) unless necessary.
* Use all lower case for class names, and separate words with hyphens &ndash; no underscores or camelCase, please.
* Keep class names short and descriptive; avoid unnecessary abbreviations. `.primary-button`, not `.pb`.
* Separate presentational and behavioral class names. Prefix JavaScript hooks with `js-`, and never use these in the CSS &ndash; if you find yourself wanting to style an element with a `js-` class, add a new class name and use that instead.
* If you need JS to toggle a presentational class in CSS, it’s handy to use `is-` or `has-`, like `is-admin` or `has-icon`.

```scss
// Nope
#replyForm button.js-submit-form {
  margin: 20px;
}

// Yep
.submit-button {
  margin: 20px;
}
```


Comments
--------

**Comment early and comment often.** Keeping mental track of why you made every potentially-questionable decision is hard enough for one person, let alone several people &ndash; in code as in life.

CSS comments `/* like this */` work in Sass; but we prefer single-line comments `// like this`, since they get stripped out when the Sass gets compiled. Spacing gives you an idea about what the comment covers:

```scss
// This is a comment about a big block of stuff

.foo {
  background-color: $bg-color;
  position: absolute;
}

.bar {
  margin: 20px;
  position: relative;
}

// This comment is about just this ruleset
.baz {
  background-color: $primary-button-color;
  margin: 20px;
}

.qux {
  margin: 20px;
  position: relative; // This is a comment about what this line does
}
```

If you need to make the same comment more than once, you can also make headnotes at the top of the file and refer to them where you need to. This probably won’t happen often, and then most likely when we need to override too-specific stuff in old CSS. (All the IDs!)

```scss
// [1] Need '!important' here to override legacy stuff

.foo {
  background-color: $bg-color;
  margin-right: 0 !important; // [1]
}

.bar {
  font-size: 12px;
  margin-right: 10px !important; // [1]
  padding: 0 !important; // [1]
}
```

Generally, leaving a comment when you use `!important` is a good idea so someone, maybe even Future You, can locate and hopefully fix the offending rule that made you use it in the first place.


SCSS
----

###Nesting

Remember [The ‘Inception’ Rule](http://thesassway.com/beginner/the-inception-rule): don’t nest more than three levels deep! In a project of this complexity, *some* excessive nesting is unavoidable, but keep it in mind as you create new elements. If you have this:

```html
<ul class="posts">
  <li class="post">
    <a class="post-info">
      <span class="post-title">My awesome post</span>
      by <span class="post-author">Bloomfire Pyro</span>
    </a>
  </li>
</ul>
```

...Sass makes it easy to do this:

```scss
// Nope
.posts {
  display: block;
  list-style-type: none;
  .post {
    margin-bottom: 5px;
    .post-info {
      color: $link-color;
      .post-title {
        font-weight: 500;
      }
      .post-author {
        font-weight: 200;
      }
    }
  }
}
```

...which compiles to stuff like this:

```scss
// Nope
.posts .post .post-info .post-author {
  font-weight: 200;
}
```

There are times (especially when dealing with pre-existing elements) you’re going to have to be a little defensive with nesting; but in general, don’t be afraid to de-nest. This:

```scss
// Yep
.posts {
  display: block;
  list-style-type: none;
  .post {
    margin-bottom: 5px;
  }
  .post-info {
    color: $link-color;
  }
  .post-title {
   font-weight: 500;
  }
  .post-author {
    font-weight: 200;
  }
}
```

...compiles to this:

```scss
// Yep
.posts .post-author {
  font-weight: 200;
}
```

###Variables

Avoid using hex values for colors &ndash; use the appropriate variable from `_variables.scss` instead, even for the basics. This provides a foundation for community owners to create a custom theme; and it makes it easier for us to tweak things in the future, say if we want to change ‘white’ to [beige, off-white, stucco, ecru, or mother-of-pearl](https://www.youtube.com/watch?v=8Q2hTL2DNfY).

```scss
// Nope
.foo {
  background-color: #fff;
  color: #444;
}

// Yep
.foo {
  background-color: $white;
  color: $text-color;
}
```

###`@extend`s and placeholders

Sass has an `@extend` facility, where you can do something like this:

```scss
.button {
  background-color: $primary-button-color;
  padding: 10px 30px;
}

.secondary-button {
  @extend .button;
  background-color: $secondary-button-color;
}
```

That’s excellent for keeping code DRY, but [it can lead to pretty significant bloat if not used carefully](http://csswizardry.com/2014/01/extending-silent-classes-in-sass/).

Placeholders begin with `%` and act like classes, but don’t get compiled into the final CSS; so only use them in `@extend`. If you find yourself wanting to `@extend` a class, create a placeholder instead and add it to `_placeholders.scss`.


Credits and Additional Reading
-------------------------------

These guidelines were inspired by the following sites, all of which are worth a read:

* [Code Guide by @mdo](http://codeguide.co/)
* [CSS Guidelines](http://cssguidelin.es/) by Harry Roberts
* [Github CSS Style Guide](https://github.com/styleguide/css)
* [Google HTML/CSS Style Guide](http://google-styleguide.googlecode.com/svn/trunk/htmlcssguide.xml)
* [Idiomatic CSS](https://github.com/necolas/idiomatic-css) by Nicolas Gallagher
* [Sass Guidelines](http://sass-guidelin.es/) by Hugo Giraudel
