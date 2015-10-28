# [HouseTrip](http://www.housetrip.com) SASS-CSS Style Guide {

## Preamble: the TL,DR

On a large site the most important thing to understand is to strive for re-use as much as possible.

With that end the overall strategy of our SASS/CSS is to take some well understood (and battle-tested) functionality from bootstrap and add our components on top.

- We use a modified BEM syntax: `.parent-name--child-name_modifier`
- JS behaviour is add to elements via `data-js="js-behavior-name"`
- Use soft-tabs with a two space indentation
- Don't add to the legacy files
- No CSS in the view layer on HTML elements.
- **Pragmatism** above all things.

## <a name='TOC'>Table of Contents</a>

  1. [History](#history)
  1. [SASS](#sass)
  1. [The Styleguide](#styleguide)
  1. [Formatting](#formatting)
  1. [Guidelines](#guidelines)
  1. [HouseTrip Style](#housetrip-style)
  1. [The don'ts](#donts)
  1. [File Organisation](#file-organisation)
  1. [Off Topic](#off-topic)

## <a name='history'>History</a>

We have currently 4 main areas of SASS/CSS in our app.

* **legacy** - used for old fixed width designs. DO NOT ADD CODE HERE.
* **admin** - used for admin areas, bootstrap with customisation
* **v4** _(retired)_ - the responsive pages: home, search results & property pages
* **responsive** - Fully modern responsive replacement for v4

The long term goal is to move older pages off the legacy styles and get admin onto only one version of bootstrap, with less customisation.

**[[⬆]](#TOC)**

## <a name="sass">SASS</a> ([sass-lang.org](http://sass-lang.com/))

Our pre-processor of choice. 

Generally we prefer the indented, HAML-style SASS syntax over the CSS-style SCSS syntax.

SASS/CSS is _not_ code, but you can apply a lot the same principles to it. Knowing how it's different though, and different again on a large site, is key to producing reusable, fast and understandable styling.

SASS is 'processed' into CSS which means you have to think about the text being spat out at the end of the compile process. Behaviours that make sense in Ruby might not make sense in SASS if speed and maintainability are you goals.

**[[⬆]](#TOC)**

## <a name="styleguide">The Styleguide</a> ([housetrip.com/en/styleguide](housetrip.com/en/styleguide))

As part of the web app we have a component library, where designers and developers can examine existing components and are encouraged to examine and reuse components where they can.

Our approach is broadly Rails partial based for HTML reuse. If a component is used elsewhere in the app it should be a partial and most likely added to these documents.

There's also a section for more global formatting (forms, typography, grid) and some useful utility styles.

**[[⬆]](#TOC)**

## <a name="formatting">Formatting</a>

### Comments

Use `//` for comment blocks

```sass
// a comment
.hero
  ...
```

**[[⬆]](#TOC)**

### Use double quotes

```sass
.foo
  content: ""
```

Also quote attribute values in selectors, for consistency and code highlighting.

```sass
input[type="checkbox"]
  color: red
```

**[[⬆]](#TOC)**

### Avoid specifying units for zero-values

```sass
.foo
  margin: 0
```

**[[⬆]](#TOC)**

### Spaces after commas

Include a space after each comma in comma-separated property or function values

```sass
.bordered-thing
  border: 1px solid red, 2px dotted blue
```

**[[⬆]](#TOC)**

### Use zeros for small values

Put `0` in front of values or lengths between -1 and 1.

```sass
.button
  margin-right: 0.25em
```

**[[⬆]](#TOC)**

### Code Structure

Properties should be grouped together:

- Put at least one space after `:` in property declarations
- then layout-related properties (`position`, `float`, `display`
- then size related `width`, `height`, `margin`, `padding`
- then aspect-related properties (`color`, `border`)
- finally calls to responsive mixins, like `+respond-min($screen-sm-min)`

```sass

// Good

.container
  display: table
  position: relative

  width: 100%

  color: $color-white
  background-color: $color-gray

  +respond-min($screen-sm-min)
    height: 576px

// Bad

.container
  +when-bigger-than-mobile
    color: white
    background-color: black
  font-size: 24px
```

**[[⬆]](#TOC)**

### Pixels vs. Ems

We use `px` for `font-size`, because it offers absolute control over text, `em` is a battle for another day.

Additionally, unit-less `line-height` is preferred because it does not inherit a percentage value of its parent element, but instead is based on a multiplier of the `font-size`.

**[[⬆]](#TOC)**

## <a name="guidelines">Guidelines</a>

### Naming

There are many variants of how to name your classes, we have settled on a simple style derived from [BEM](http://bem.info) that looks like `.parent-name--child-name_modifer`.

So for example in the `components/_hero.sass` file...

```sass
.hero
  ...
.hero--background
  ...
.hero--content
  ...
.hero--title
  ...
.hero--title_left
  ...
```

Modifier or variant classes are designed to be used with their un-modified version. So if you were creating a left aligned hero title you'd use...

```haml
.hero--title.hero--title_left
  Content Goes Here
```

or

```html
<div class="hero--title hero--title_left">Content Goes Here</div>
```

In general, use semantic naming.

You may notice a few places where variants of a class use a color-based naming. For example `block.block_blue` in the `components/_block.sass`. This is because that's exactly what the variant is: "a block with a blue background".

If you have a choice, be semantic, but if the style is purely visual, name it that way.

### <a name="js-naming">JS naming</a>

Use the HTML5 `data-js` attribute to add js behaviour to an element. This helps seperate behaviour layer from the presentation and content layers of the app

```haml
// good
.map-canvas{ :data-js => "initalize-map"}

// bad
.js-map-canvas.map-canvas
```

**[[⬆]](#TOC)**

### Semantics and Fear of Classes

A lot has been written on the value of semantics to HTML and CSS a lot of this thinking is derived (rightly) from getting us out of the "tables for layout" era. We're out, we made it.

Recent experience across the industry, in writing CSS for large scale websites, has forced some re-examination of the lust for minimal classes.

So remember, do not fear multiple classes on elements.

Our choice of layout grid system (bootstrap) and some of our utility classes are inherently 'unsemantic' but it's designed for reuse of CSS and agility in putting pages together.

**[[⬆]](#TOC)**

### Adding Third Party CSS

Goes in `vendor` directory, unedited. If there are multiple files, use a directory.

Most included files will be css files so change the extension to `.scss`. If you _do_ have to make changes, keep them small (this is most likely changes to image paths) and do it in a seperate commit.

I prefer to include non-minified source, so that future changes to plugins can be easily seen in a `git diff`.

For example the mixins from [bootstrap-sass](https://github.com/twbs/bootstrap-sass/) go into `vendor/bootstrap/_mixins.scss`.

**[[⬆]](#TOC)**

## <a name="housetrip-style">HouseTrip Style</a>

We use a tweaked bootstrap (v3) grid to layout our pages.

The [bootstrap documentation](http://getbootstrap.com/css/#grid) is the best place to read up on how that works. We can use all of the existing types of bootstrap grid classes: e.g. `col-sm-offset-X`, `.col-sm-push-3`.

**[[⬆]](#TOC)**

### Special case div-itis avoidance

When you are simply after the full width of a container, there is no need to do this...

```haml
.row
  .col-sm-12
    %h1
```

You can simply omit the `row` and `col-sm-12`

```haml
%h1
```

Voila.

**[[⬆]](#TOC)**

### Components should flex within the grid element they are inside

A component should be styled independantly of the containing `col-sm-X` `div`. Overall width of a component is provided by the grid, elements within your component can be positioned within _that_ context.

Use floats and absolute positioning where required. You shouldn't need to do anything crazy like using negative margins, if you do find yourself doing it, there's probably another way.

You can also use nested `.row` and `.col-sm-X` classes within your components for additional grid-based layout.

It's also a possible smell if you find yourself setting widths rather than relying on the grid.

**[[⬆]](#TOC)**

### SVG

For all non-photographic images we should try and use SVG. It is XML-based and therefore textand this compresses extremely well.

We can also inline SVG to reduce HTTP requests as we move away from any support other than IE8.

**A note on icon-fonts**

Icon fonts are great. Including *all the icons* is not great. Generating classes for all the icons you need is a recipie for exventual CSS bloat.

We currently have the glyphicons fonts included in v4, but we use only a handful for their stated purpose (tiny icons next to text) and spend a lot of time in CSS tweaking their position.

In the updated _responsive_ layout update we are moving away from using icon fonts in favour of SVG

**[[⬆]](#TOC)**

### Colors

We have all the housetrip brand colours in `global/_colors.sass`, use these variables. Use from the top of that file and don't add any more, we are trying to reduce the number of colors and color variables.

For black and white use `#000` and `#fff`.

Prefer to use use functions (`transparentize`, `lighten`) over RGBA. You'd probably get the math wrong, and it'd be less readable, so why bother?

**[[⬆]](#TOC)**

## <a name="donts">The don'ts</a>

### Do not style ids

CSS `#something` values have very high [specificity](http://www.w3.org/TR/selectors/#specificity). Style with classes. In fact, use `#id`s as sparingly as possible.

**[[⬆]](#TOC)**

### Do not style JavaScript classes

**This is the old way of adding JS (see [JS naming](#js-naming))**

We had a convention of using `.js-hook-for-behaviour` classes to target JS. Don't style these, even if if means creating 'parallel' classes for styling.

**[[⬆]](#TOC)**

### Do not use images for gradients

There's no need when we have CSS gradients at our disposal that work across all major browsers, including IE.

**[[⬆]](#TOC)**

### Do not transform text server side

Think of the user who wants to copy and paste that text, or the next developer who needs to change the style. Implement all-caps with `text-transform:uppercase`, not (in Ruby) `String#upcase`.

**[[⬆]](#TOC)**

### Do not use `!important`

It's like leaving a loaded gun lying around. Think you have a counter-example? You're wrong. Don't say we didn't tell you.

**[[⬆]](#TOC)**

### Do not support <IE8

**Don't.** IE8 is next on the hit list.

We have officially dropped support for these old browsers and are prompting people to update. We really do not want to spend our time fixing and testing for this small sebset of our userbase.

**[[⬆]](#TOC)**

### Do not prefixed properties

Do not use browser-specific, prefixed properties directly.

We will implement [autoprefixer](https://github.com/ai/autoprefixer) into our asset pipeline.

**[[⬆]](#TOC)**

### Do not nest classes in CSS/SASS

Once you start nesting it is very easy to get into specificty nightmares. Rules surprisingly overwriting each other, leading to more specificity in the code you're writing or even the dreaded `!important` declaration.

It's often used to give context to your components...

```sass
      
// Good

```sass
.landing-page
  ...
.search-box
  ...
.search-box--title
  ...

// Bad  

.landing-page
  ...
  .search-box
    ...
    > h2
      ...
```

Try and think *in generalities* when naming. Avoid `.button-for-search-filters`, use `.button-tiny`, there might even be one already you can use.

If there isn't, don't slavishly create another kind of something, work with your designer to try and reuse existing styles.

**[[⬆]](#TOC)**

### Do not style html elements other than globally

Use descriptive classes.

```sass

// Good

.hero
  ...
.hero--title
  ...

// Bad

.hero
  ...
  h2
    ...
```

Encourages reuse and lack of surprises. And allows our 'base' styling to remain consistant.

**[[⬆]](#TOC)**

### Do not style typography in your component CSS

Most often you'll want to change the typography of something within your element. We have the somewhat ugly-but-effective `.text-l` to `.text-xxl` styles which you can apply to change those around and keep to our typographic hierarchy.

i.e. `<p class="text-xxl">` has the typographic styling of an `<h1>`

Yes it's a bit ugly, but it contains the proliferation of margins, paddings, font-sizes & line-heights across the CSS. It also helps us stick to our typographic grid.

**[[⬆]](#TOC)**

### Do not use SASS extend

You shouldn't use `extend` it's cognitively tricky and can produce huge style sheets. Particularly avoid any use of `@extend` that references a class from other file because it couples their compilation.

SASS is _generated_ into CSS, `extend` often ends up with CSS rules repeated for the selector. Just use the classes in the HTML. _Keep it simple._

e.g.

```sass
.font-size-xxl
  font-size: 600px

.hero-title
  @extend .font-size-xxl
  text-shadow: 10px solid #000
```

becomes

```css
.font-size-xxl, .hero-title {
  font-size: 600px;
}
.hero-title {
  text-shadow: 10px solid #000
}
```

The extend is an indirection and rapidly becomes complex when extend is used in multiple places. It's simpler for 'future you' to get the same result by doing this in your HTML. This is what the _cascade_ in Cascading Style Sheets is for.

```sass
.font-size-stupid
  font-size: 600px

.hero-title
  text-shadow: 10px solid #000
```

```html
<h2 class="font-size-stupid hero-title">Mega Title</h2>
```

**[[⬆]](#TOC)**

### Do not nest imports

... are confusing. Don't do it.

The top-level stylesheets should include all required `@import` directives and be well commented.

The only dependancies for a component should be the 'global' styles: typography, colours etc. These will be included by any top-level stylesheet.

**[[⬆]](#TOC)**

### Do not reinvent the wheel

Look at the component library. Look at other pages. Are there elements you could re-use?

Talk with your designers. Can we use this element that already exists rather than introduce a new variant?

Only use variables when they are truly global. Then put them in `global/_variables`.

**[[⬆]](#TOC)**

### Page specific styles

There is *never* a good reason to write page-specific styles, so `pages` should be empty. If it isn't, each file name should be the snake-cased, full path to the corresponding view partial.

**[[⬆]](#TOC)**

## <a name="file-organisation">File Organization</a>

```
<app/assets/stylesheets>
├── application.sass               # app spreadsheet
├── legacy_application.sass        # don't add stuff in here
│
├── components                     # reusable elemenets
│   ├── _badges.css.sass
│   ├── _block.sass
│   ├── _footer.sass
│   ├── _hero.sass
│   ├── _highlight.sass
│   ├── ...
│   └── _navigation.sass
│
├── global
│   ├── _colors.sass               # colors
│   ├── _fonts.sass                # font-face
│   ├── _responsive_mixins.sass    # mixins
│   ├── _typography.sass           # typography
│   └── _variables.sass            # fonts
│
├── vendor                         # vendored (as scss files)
│   ├── _normalize.scss            # normalize cross-browser
│   ├── _jquery.plugin.scss        # js plugin css
│   └── bootstrap                  # bootstrap scss files
│       ├── _forms_variables.scss  # our tweaks to the vars
│       ├── _forms.scss
│       ├── _grid_variables.scss   # our tweaks to the vars
│       ├── _grid.scss
│       ├── _attachments.scss
│       └── ...
│       └── _utilities.scss
│
├── legacy						   # HERE BE DRAGONS
│   ├── ...
│   └── _lots.sass
│
├── v4   						   # working on moving into
│   ├── ...                        # the new coding standards
│   └── ...
│
└── pages                          # view specific partials
      └── controller_name            # try not to do this
          ├── _index.scss
          └── _show.scss
```

**[[⬆]](#TOC)**

## <a name"off-topic">Off Topic</a>

### Emails

_We are moving to responsive emails... please speak to your lead developer if you have email related stories._

The rules above apply, mostly. We have CSS inliners (`premailer` in Rails 2) that mostly do the heavy lifting for you.

The notable exception being, of course, that most of your layout will be done with tables.

Avoid using floats, padding, and margins in emails.

**[[⬆]](#TOC)**

-------------------------------------------------------

Thanks for reading!

If you disagree with something, or think the guide lacks something, feel free to issue a pull request against the guide!
Be prepared to defend, as we usually merge only when a consensus is reached.
