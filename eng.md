<article class="post-260508 post type-post status-publish format-standard has-
post-thumbnail hentry category-coding tag-css
">
For some time, we’ve run up against the **limits of what CSS can do**. Those
who build responsive layouts will freely admit the frustrations and shortcomings
of CSS that force us to reach for CSS preprocessors, plugins and other tools to 
help us write the styles that we’re unable to write with CSS alone. Even still, 
we run into limitations with what current tools help us accomplish.

Think for a moment of a physical structure. If you’re building a large
edifice with weak material, a lot of external support is required to hold it 
together, and things have to be overbuilt to stay sturdy. When you’re building a
website out of HTML, CSS and JavaScript, this external support might look like 
frameworks, plugins, preprocessors, transpilers, editing tools, package managers
and build processes.

Instead of adding yet another plugin to the top of the stack, I wondered
whether, by**extending one of the core languages, CSS**, we could strengthen
the material that websites are built from, developing better, stronger websites 
that require less external support and tools to build.

### The Current State Of Affairs {#the-current-state-of-affairs}

With tools such as CSS preprocessors, we write CSS in shorthand, to be expanded
later into its full form (before being viewed in a browser). Plugins are able to
operate on the page alongside the elements they affect, but to apply styles, 
they either write CSS styles directly to HTML or toggle class names that apply 
different CSS rules. In both cases, we need to write or generate the CSS that we
will need before the page actually loads.

#### The Problem {#the-problem}

The problem with this method is that even the best plugins often **require
customization and configuration** in each layout you use. Also, when JavaScript
is writing styles for you, it can be hard to keep your plugin-based logic and 
your CSS styles together when refactoring or reusing code.

Another problem with preprocessors is that any errors written in shorthand
quickly balloon into a much larger mess once the CSS is expanded into its full 
form. When using plugins, we add many potential points of failure. We may be 
using multiple plugins to accomplish a handful of different things that might 
all be unnecessary if CSS were a little more powerful. This creates an extra 
burden for developers to maintain, for browsers to render and for users to 
download.

#### Is There Any Hope for the Future of Web Development? {#is-there-any-hope-
for-the-future-of-web-development
}

In 2013, Tyson Matanich wrote an article titled “
[Media Queries Are Not the Answer: Element Query Polyfill][1][1][2],” which
introduced the concept of element queries to a large audience. It kicked off a 
discussion about how plugins and polyfills could be built to navigate around the
shortcomings of CSS.

Since then, while we wait for CSS features to advance, a number of plugins have
been released that allow developers to use element queries in a few different 
ways.

#### What Are Element Queries? {#what-are-element-queries}

Element queries are like media queries, except that their rules apply to the
properties of actual elements, rather than those of the browser’s viewport.

### How EQCSS Came About {#how-eqcss-came-about}

In late 2013, I found myself working on the front end of a Ruby on Rails web
app. The app needed to display detailed information to users, and the goal was 
to build a responsive interface that would perform equally well on phones, 
tablets and desktop browsers. This posed a few challenges, one of which was that
much of the important content to be displayed was best displayed in tables — yes,
actual`table` elements (to show financial transactions, sports records, etc
.).<figure>

![][3][2][4]  
<figcaption>The EQCSS project came to be as a result of research on element
media queries. Now, it’s[finally released][5][3][6], and you can use it today
. [Check the demos.][7][4][8]</figcaption></figure>
I created responsive styles using media queries that displayed the `table`
element correctly for browsers of different sizes. But as soon as one of those 
responsive tables was displayed in a template that contained a sidebar, suddenly
all of my responsive breakpoints turned into responsive*broke*points. They
simply didn’t account for the 200-pixel-wide sidebar, causing things to overlap 
and appear broken.

Another obstacle: User names varied in length from 3 to 20 characters, and I
found myself wishing I could automatically**adjust the font size of each user
name based on the number of characters** it contained. I needed to put each
user name in the sidebar, and it was tricky to pick a font size small enough to 
fit a 20-character user name but large enough for the viewer to see a 3-
character user name.

To work around problems like these, I would often find myself copying entire
media queries, duplicating large sections of my code base, simply because I 
needed a smarter way to apply the responsive styles in each layout. I relied on 
JavaScript as another makeshift solution, writing many nearly identical 
functions that would watch a page and apply styles in places where CSS couldn’t 
reach. After a while, the added burden of all of this duplicate code began to 
weigh down the code base and made changes difficult to do.

I knew there had to be a better solution, and after a while I began to think: I
don’t need media queries — what I need are element queries!

#### Research and Development {#research-and-development}

Through 2014, I began experimenting with different ways to inform CSS about the
properties of elements as they appeared on the page, so that I could apply 
better styles. I was hoping to discover an approach that would allow me to write
styles that combined the beauty of CSS with the power of JavaScript.

Some abandoned approaches I gave up on include adding attributes to HTML tags
in order to add responsive support, and trying to find ways to embed entire 
blocks of CSS code inside of JavaScript-based`if` statements to produce some
sort of Frankenstein monster patched together from JavaScript and CSS.

But instead of making things easier, all of my failed approaches had one thing
in common: They added more work! I knew that the right solution would simplify 
and reduce the work that needs to get done, so I kept searching. Through these 
experiments, I ended up with a refined idea of the type of syntax that would be 
required for element queries to work well.

As mentioned, for a website built from HTML, CSS and JavaScript, external
support comes in the form of frameworks, plugins, preprocessors, transpilers, 
editing tools, package managers and build processes. Instead of adding yet 
another plugin to the top of the stack, I wondered whether, by extending one of 
the core languages, CSS, we could strengthen the material that websites are 
built out of, building better, stronger websites that require less external 
support and tools to build.

#### The Birth Of A Syntax {#the-birth-of-a-syntax}

By the end of 2014, equipped with a better vision of the syntax needed, I
contacted Maxime Euzière, a phenomenal JavaScript code golfer and asked his 
opinion about the possibility of extending CSS using JavaScript in the browser 
at runtime. Not only did he inform me that it is possible, but he offered to 
help me do it! We named the syntax EQCSS, short for “element query CSS.” The 
name is also a nod to the word “excess,” because everything it does is in excess
of what CSS can do.

#### The Need {#the-need}

My requirement for the syntax was that it be **as close to CSS** as possible
— so close that syntax highlighters would be fooled into thinking it was 
standard CSS. So, I mapped out the CSS syntax for element queries that made 
sense — the kind of syntax that people are surprised doesn’t exist already.

I knew that if we were going to extend browser support for CSS using JavaScript
, then the plugin would need to be as lightweight and straightforward as 
possible to get the job done, which ruled out using libraries such as jQuery to 
build the plugin. I needed a pure JavaScript library that would add the features
I want in the future into the browsers that I have to support today.

At this time, discussion in the CSS community is focused on custom `@` rules,
and talk about element queries is[still preliminary][9][5][10]. We are likely
still years away from any official CSS specification for features like this, and
even after a specification, we will still need to wait for enough browser 
support before we can use those features in websites.

Waiting for these features to be added to CSS doesn’t make sense when we need
this functionality to build and fix websites today.

#### The Result {#the-result}

The result of this research has been the creation of a syntax that includes a
new set of advanced responsive conditions, scoped styles and new selectors for 
targeting elements, as well as a pure JavaScript library named[EQCSS.js][11]
[6][12]. Plus, support for Internet Explorer (IE) 8 has been provided in an
optional external polyfill. Both the plugin and polyfill have been released 
under the[MIT license][13][7][14] and are free for everyone to use.

### Use Cases For Element Queries {#use-cases-for-element-queries}

#### Plugin Developers {#plugin-developers}

When creating UI components and widgets, developers often find themselves
limited by media queries. We often have to choose between building many 
different layouts that can be configured by the person using the plugin, and 
simplifying the interface to the point that you can build a one-size-fits-most 
solution.

But when designing plugins and interfaces with element queries, we can easily
write responsive styles that cover all of the situations we anticipate, making 
them truly bulletproof, no matter what content the user puts inside or where the
plugin shows up. Suppose we could style a widget with layouts ranging from 150 
to 2000 pixels wide. Then, no matter where that widget is displayed on a website,
it would always look great.

#### Template Builders {#template-builders}

When you’re prototyping a website, it’s common to reorganize design
elements on the page and to think about the design as a collection of modular 
components. If you’ve written CSS media queries, sometimes this can be a case of
**premature optimization**. By designing with element queries, you keep
responsive conditions layout-independent, giving you a lot more flexibility to 
move things around without having to rework styles as much.

Things I have found especially useful to have designed or mocked up using
element queries include:

*   navigation bars,
*   modals,
*   sign-up and log-in forms,
*   footers,
*   pricing charts,
*   landing pages,
*   tables,
*   tab boxes,
*   accordions,
*   sidebar widgets,
*   media players,
*   testimonial sections.

Any design element can be “scoped” and ported anywhere — page to page or
website to website.

#### Device Support {#device-support}

One of the problems you’re faced with when supporting the web on mobile
devices is the abundance of hardware. The market for devices is more fragmented 
than ever before, and new devices are appearing every day. We can no longer 
maintain a list of the browsers and devices we support, so it’s crucial to know 
that a design works everywhere, even on devices that haven’t been released yet.

By using element queries, you can design websites in a better way and eliminate
some of these cross-browser differences.

Many articles written recently about the need for element queries illustrate
many of the use cases in detail. So, let’s get on with how to use them!

### How To Write Element Queries {#how-to-write-element-queries}

Getting started with EQCSS is easy. All you need to start using EQCSS syntax is
to[include the JavaScript][15][8][16] somewhere in your HTML.

#### Downloading EQCSS.js {#downloading-eqcss}

If you want to clone the EQCSS project from GitHub, you can type:

    git clone https://github.com/eqcss/eqcss.git

If you use npm, you can add EQCSS to your project with the following command:

    npm install eqcss

#### Adding EQCSS.js to Your HTML {#adding-eqcss-to-your-html}

Once you have downloaded EQCSS, you can add it to your HTML with a `script` tag
:

    <script src="EQCSS.js"></script>

This file (`EQCSS.js`) includes support for all current browsers, including IE
9 and up. To support IE 8, we would have had to have used a lot of other 
polyfills. Keep in mind that IE 8 doesn’t even support CSS media queries without
a polyfill, so it’s pretty amazing that we were able to get element queries 
working there as well. To include IE 8 support for a website using EQCSS, add 
the following link before your link to the main plugin:

    <!‐‐[if lt IE 9]><script src="EQCSS‐polyfills.js"></script><![endif]‐‐>

#### Running EQCSS {#running-eqcss}

By default, the EQCSS plugin will compute any styles it finds once the page
loads, and also whenever it detects the browser being resized, similar to media 
queries. You can also call`EQCSS.apply()` manually with JavaScript to
recalculate styles at any time, which can be useful once content has been 
updated on the page.

#### Writing Element Query CSS {#writing-element-query-css}

The EQCSS.js plugin can read styles in a couple of different ways. You can
include EQCSS in any`style` tags on an HTML page. You are also able to write
EQCSS in an external CSS style sheet.

If you would like to keep your EQCSS-powered code separate from your CSS, you
can load EQCSS syntax using the`script` tag with the type set to `text/eqcss`.
You can add styles inline in a tag like this, or link to an external`.eqcss`
style sheet with
`<script type="text/eqcss" src=styles.eqcss></script>`, which would
load a file named`styles.eqcss`.

### Anatomy Of An Element Query {#the-anatomy-of-an-element-query}

#### Style Scoping {#style-scoping}

The syntax in EQCSS for writing element queries is very similar to the
formatting of CSS media queries, but instead of`@media`, we begin the query
with`@element`. The only other piece of information we need to supply is at
least one selector that these styles should apply to. Here’s how you would 
create a new scope for an element named`<div class="widget">`:

    @element '.widget' {
    
    }

The element between the quotes (in this case, `.widget`) may be any valid CSS
selector. With this query, we have created a new scope on the`.widget` element
. We haven’t included any responsive conditions for the scope yet, so any styles
in a query like this would apply to the scoped element at all times.

Without the ability to scope styles to one or more elements (instead of the
whole page at once), we wouldn’t be able to apply responsive queries to just 
those elements. Once we have created that element-level scope, using the more 
advanced features of the EQCSS syntax becomes easy — like the`$parent` meta
selector, for example — because JavaScript now has a reference point from which 
to calculate things like the`parentNode` of the scoped element. This is huge!

True, CSS already includes a direct-descendant selector, with the `>`, which
allows us to select elements that are direct children of the specified element. 
But CSS currently offers no way to travel in the other direction up the family 
tree, to select an element that contains another element, which one would call 
its parent element. The “CSS Selectors Level 4” specification now includes a
[`:has()` selector][17][9][18], which works in similar way to jQuery’s 
[`:has()` selector][19][10][20], but currently [browser support is nil][21]
[11][22]. Scoped CSS makes a different kind of parent selector possible.

Now that we have opened a scope in the context of the `.widget` element, we can
write styles from its perspective to target its own parent element:

    @element '.widget' {
      $parent {
        /* These styles apply to the parent of .widget */
      }
    }

Another example of special selectors that can be used in any element query are
the`$prev` and `$next` selectors, which represent the previous and next sibling
elements. Even though CSS can reach the next sibling of our widget with a 
selector like`.widget + *`, there’s no way in CSS to reach backward and
select the element that comes directly before another element.

    <section>
      <div>This will be the previous item</div>
      <div class="widget">This is the scoped element</div>
      <div>This will be the next item</div>
    </section>
    <style>
      @element '.widget' {
        $prev {
          /* These styles apply to the element before .widget */
        }
        $next {
          /* These styles apply to the element after .widget */
        }
      }
    </style>

#### Element Queries {#element-queries}

Developers most frequently use CSS media queries for responsive design by
applying styles based on the height or width of the browser’s viewport. EQCSS 
syntax supports many new types of responsive conditions. Instead of working with
the width and height of the browser alone, you can write styles that apply to 
elements based on their own properties, like how many child elements it contains,
or how many characters or lines of text are in the element at the moment.

Adding these responsive conditions to your scoped styles is similar to how you
would format media queries: You’d add`and (condition: value)` for each
condition you want to check. In this example, we will check whether any
`.widget` elements are displaying at least 500 pixels wide on the page.

    @element '.widget' and (min‐width: 500px) {
      /* CSS rules here */
    }

The syntax of an element query breaks down as follows:

*   **element query**  
    `@element selector_list [ condition_list ] { css_code }` 
*   **selector list**  
    `" css selector [ "," css selector ]* "` 
*   **condition list**  
    
    `and ( query_condition : value ) [ "and (" query condition ":" value ")" ]*`
   
*   **value**  
    `number [ css unit ]` 
*   **query condition**  
    
    `min-height | max-height | min-width | max-width | min-characters | max-characters | min-lines | max-lines | min-children | max-children | min-scroll-y | max-scroll-y | min-scroll-x | max-scroll-x`
   
*   **css unit**  
    `% | px | pt | em | cm | mm | rem | ex | ch | pc | vw | vh | vmin | vmax`
   

As another example, here’s how to write a query that turns the `body` element
red when the`.widget` element reaches 500 pixels wide:

    @element '.widget' and (min‐width: 500px) {
      body {
        background: red;
      }
    }

Note that the `body` element changes when the `.widget` reaches a certain width
, not the`.widget` element itself!

### Element Query Conditions {#element-query-conditions}

Below is the full list of responsive conditions that EQCSS supports.

#### Width-Based Conditions {#width-queries}

#### Height-Based Conditions {#height-queries}

#### Count-Based Conditions {#count-queries}

You can combine any number of these conditions in your element queries for
truly multi-dimensional responsive styles. This gives you much more flexibility 
and control over how elements render. For example, to shrink the font size of a 
header that has more than 15 characters when less than 600 pixels of space is 
available to display, you could combine the conditions for`max‐characters: 15`
and`max‐width: 600px`:

    h1 {
      font‐size: 24pt;
    }
    @element 'h1' and (min‐characters: 16) and (max‐width: 600px) {
      h1 {
        font‐size: 20pt;
      }
    }

One of the problems you might run into once you start writing scoped styles
with responsive conditions is that, when multiple instances of the same selector
are on a page, using that selector in your element query will apply styles to*
all* instances of that selector on the page when *any* of them match the
conditions. Taking our`.widget` examples from earlier, suppose we had two
widgets on the page (maybe one in the sidebar and another displaying at full 
width) and we wrote our element query like this:

    @element '.widget' and (min‐width: 500px) {
      .widget h2 {
        font‐size: 14pt;
      }
    }

This would mean that when *either* `.widget` element on the page is at least
500 pixels wide, the style would apply to both`.widget` elements. This is
probably not what we want to happen in most cases. This is where meta selectors 
come in!

The two parts that make up our element query are the selector and the
responsive condition. So, if we want to target only the elements on the page 
that match both the selector*and* the responsive conditions at the same time,
we can use the meta selector`$this`. We can rewrite our last example so that
the style applies only to`.widget` elements that match the `min‐with: 500px`
condition:

    @element '.widget' and (min‐width: 500px) {
      $this h2 {
        font‐size: 14pt;
      }
    }

A number of new selectors are supported by the EQCSS syntax that aren’t in
regular CSS. Here is the full list:

*   `$this`  
    [demo][23][32][24] 
*   `$parent`  
    [demo][25][33][26] 
*   `$root`  
    [demo][27][34][28] 
*   `$prev`  
    [demo][29][35][30] 
*   `$next`  
    [demo][31][36][32] 

These selectors will only work in an element query.

#### Opening the Portal Between JavaScript and CSS {#opening-the-portal-between
-javascript-and-css
}

The last and final feature of EQCSS is the wildest of all: `eval('')`. Through
this doorway lies all the power of JavaScript, accessible from CSS. Though 
JavaScript can apply styles to elements, it currently has a hard time reaching 
some things, such as the`:before` and `:after` pseudo-elements. But what if CSS
could reach JavaScript from the other direction? Instead of JavaScript setting 
CSS properties, what if CSS could be aware of JavaScript?

This is where `eval('')` comes in. You can access and evaluate any JavaScript,
whether to use the value of a JavaScript variable in your CSS, to execute a 
JavaScript one-liner (like`eval('new Date().getFullYear()')`), to ascertain
values about the browser and other elements that JavaScript can measure (like
`eval('innerHeight')`) or to run a JavaScript function and use the value it
returns in your CSS styles. Here’s an example that outputs`© 2016` in a 
`<footer>` element:

    @element 'footer' {
      $this:after {
        content: "© eval('new Date().getFullYear()')";
      }
    }

While evaluating JavaScript, `eval('')` also works from the context of the
actively scoped element, the same element to which styles for`$this` apply. You
can use`$it` in JavaScript written in `eval('')` as a placeholder for the
scoped element if it helps you to structure the code, but if omitted, it will 
work the same way. For example, let’s say we have a`div` with the word “hello
” in it. The following code would output “hello hello
”:

    <div>hello</div>
    <style>
      @element 'div' {
        div:before {
          content: "eval('$it.textContent') ";
        }
      }
    </style>

Here, `$it` refers to the `div` because it’s the currently scoped selector.
You could also omit the`$it` and write the following code to display the same
thing:

    @element 'div' {
      div:before {
        content: "eval('textContent') ";
      }
    }

`eval('')` can be helpful in situations where CSS isn’t aware of measurements
or events that happen on the page after it has loaded. For example, iframe 
elements used for embedding YouTube videos come with a specified width and 
height. While you can set the width to`auto` in CSS, there’s no easy way to
maintain the correct aspect ratio of width to height when the video expands to 
fill the available space.

A common workaround for maintaining responsive aspect ratios while scaling is
to place the content that needs to maintain its aspect ratio in a wrapper, and 
then add padding to the wrapper with a value based on the ratio of width to 
height that you want to maintain. This works, but requires you to know the 
aspect ratio of all videos in advance, as well as requires more HTML markup (a 
wrapper element) for each video that you want to embed responsively. Multiply 
that across every website that has responsive videos, and that’s a lot of cruft 
that wouldn’t need to be there if CSS were just a little smarter.

Maybe a better approach to responsive aspect ratios involves placing each video
in a wrapper without padding and then writing a JavaScript library that 
calculates the aspect ratio of each video it finds and applies the correct 
amount of padding to each wrapper.

But what if CSS *could* access those measurements directly? Not only could we
consolidate the CSS for all different aspect ratios into one rule, but if we 
could evaluate it once the page loads, we could write one rule that responsively
resizes any video we ever give it in future. And we could do it all without any 
wrappers!

If this seems too good to be true, check this out. Here’s how simple it is to
write wrapper-free responsively resizing iframe elements in EQCSS:

    @element 'iframe' {
      $this {
        margin: 0 auto;
        width: 100%;
        height: eval('clientWidth/(width/height)');
      }
    }

Here, `width` and `height` refer to the `width=""` and `height=""` attributes
in the iframe in HTML. JavaScript can perform the calculation of
`(width/height)`, which gives us the aspect ratio; and to apply it at any width
, we would just divide the current width of the iframe element by the ratio.

This has the succinctness and legibility of CSS, and all the power of
JavaScript. No extra wrappers required, no extra classes and no extra CSS.

Be careful with `eval('')`, though. [There’s a reason][33][38][34] why CSS
expressions were considered dangerous in the past, and there’s also a reason why
we tried out the idea. If you aren’t careful with how many elements you are 
applying them to on the page or with how frequently you are recalculating styles,
then JavaScript could end up running things hundreds of times more than needed. 
Thankfully, EQCSS allows us to invoke`EQCSS.apply()` or `EQCSS.throttle()` to
recalculate the styles manually, so that we have more control over when styles 
are updated.

#### The Danger Zone! {#the-danger-zone}

Other problems can occur if you create queries with conflicting conditions or
styles. EQCSS, like CSS, is read top to bottom using a
[hierarchy of specificity][35][39][36]. Although CSS is a declarative language
, it does contain some advanced features. It’s only a couple steps away from 
being Turing-complete as a programming language. So far, debugging CSS has been 
a pretty straightforward affair, but EQCSS shifts CSS from being simply an 
interpreted declarative language into being a**dynamic style sheet language**
with an additional layer of interpretation between CSS and the browser. With 
this new territory comes a variety of potential new pitfalls.

Here’s an example of a reciprocal loop in EQCSS, something that normal CSS
media queries are immune to by design:

    @element '.widget' and (min‐width: 300px) {
      $this {
        width: 200px;
      }
    }

I call this `jekyll: hide;` CSS. But in addition to one style continually
triggering itself, there’s also the possibility of writing multiple queries that
trigger each other, in what we call a “double-inverted reciprocal loop,” the 
nastiest of all:

    @element '.widget' and (min‐width: 400px) {
      $this {
        width: 200px;
      }
    }
    @element '.widget' and (max‐width: 300px) {
      $this {
        width: 500px;
      }
    }

In theory, that unfortunate widget would be stuck in a loop, resizing between
200 and 500 pixels until the end of time, unable to settle on a width. For cases
like this, EQCSS simply computes the rules in the order they are evaluated, 
awards the winner and moves on. If you were to rearrange the order in which 
these rules appear, the latter style would always win if they are of equal 
specificity.

Some people say that the ability to create loops (or even double-inverted
reciprocal loops) is a design flaw, but in order to prevent loops from being 
possible, you would need to limit the power of EQCSS in a way that would remove 
most of the value that the syntax provides. On the other hand, creating infinite
loops in JavaScript is very easy, but that’s not viewed as a flaw of the 
language — it’s seen as evidence of its power! It’s the same with element 
queries.

#### Debugging Element Queries {#debugging-element-queries}

Currently, debugging element queries can feel a bit like debugging media
queries before we had tools like the web inspector to show us the styles as they
were computed on the page. For now, debugging and developing element queries 
require the developer to maintain a mental model of what responsive behavior 
should be happening. In the future, building EQCSS-aware developer tools might 
be possible, but as of now, the developer tools in all major browsers are only 
aware of the styles that EQCSS has already applied to the elements on the page, 
and they are not aware of the responsive conditions that EQCSS is watching.

### How To Design With Element Queries {#how-to-design-with-element-queries}

The simplest way to make use of element queries is to convert existing designs
using media queries into element queries, “liberating” elements and their 
responsive styles from one layout and making it easy to reuse that code in other
pages and projects. The following media query and element query could mean the 
same thing:

    footer a {
      display: inline-block;
    }
    
    @media (max‐width: 500px) {
      footer a {
        display: block;
      }
    }

    footer a {
      display: inline-block;
    }
    
    @element 'footer' and (max‐width: 500px) {
      $this a {
        display: block;
      }
    }

The difference is that, in the original example, the footer links stay as 
`display: block` until the *browser* is at least 500 pixels wide. The second
example, using element queries, would look the same but only if the`footer`
element was full width.

After liberating this style from its original media query, we can now place the
footer in a container of any width and be sure that when the footer needs the 
responsive style to be applied (i.e. when it is narrower than 500 pixels), it 
will be applied.

1.  Ensure that `EQCSS.js` is present in the destination document’s HTML.
2.  Replace `@media` with `@element` in the CSS.
3.  Add a CSS selector to the scope of each `@element` query.
4.  Optional: Replace occurrences of the scoped element with `$this` in element
    queries.
   

Unless the design component you are converting was originally designed to
display using the full width of the browser’s viewport, you will likely have to 
tweak the breakpoints after converting to element queries.

### Avoiding Lock-In {#avoiding-lock-in}

The experience of authoring EQCSS styles is similar to the experience of
writing regular CSS: All of your favorite CSS properties and techniques are 
still there, just with some additional features that help them work together in 
new ways. Because EQCSS outputs standard CSS to the browser, any CSS feature 
that your browser has built-in support for will work when used with EQCSS. If 
someday features such as element queries and scoped styles are specified in CSS 
and supported in browsers, then you would be able to begin using them in your 
EQCSS code right away, while still relying on EQCSS for the other features that 
the browser doesn’t yet support natively.

Because you can use EQCSS syntax both directly in CSS as well as in its own
script, with the type`text/eqcss`, if CSS ever develops a syntax for native
element queries, you would still be able to load EQCSS as a script and avoid 
conflicts.

Looking forward, one solution that browser developers are experimenting with
right now is[Houdini][37][40][38], which would open up access for plugin
developers to extend CSS in new ways, like adding support to the browser itself.
Some day, it might be possible to write more efficient plugins that interpret 
EQCSS syntax and bring these features to browsers in a more direct and 
performant way than the current JavaScript library allows.

Yes, even though element queries provide many new and exciting ways to target
elements with styles, media queries (though limited) will always run faster in 
the browser than a style that relies on JavaScript to compute. However, CSS 
media queries are for more than just styling HTML for screens. CSS media queries
also support print-based queries and other ways that websites display 
information. EQCSS can be used in conjunction with media queries for things like
print styles, so there’s no need to retire the media query just because element 
queries can now also be used!

### Designing With 20/20 Vision {#designing-with-20-20-vision}

The best thing we can do for the future of CSS is to experiment with these
ideas as much as possible today. No amount of brainstorming and theorizing about
these features will be as useful as actually trying to implement them and put 
them to use, discovering the techniques that make them useful and powerful.

In addition to providing a solution for element queries, EQCSS.js hopefully
also serves as a platform for other experiments in extending CSS. If you have an
idea for a new responsive condition, a new CSS property or a new selector to use
in your code, forking EQCSS.js and modifying it to include your idea can get you
most of the way there with little effort.

### Modular Design {#modular-design}

In designing layouts using element queries, the biggest shift is learning how
to stop viewing the DOM from the top down and from the perspective of only the 
root HTML element, and to start thinking about individual elements on the page 
from their own perspectives within the document.

The old paradigms of “desktop-first” and “mobile-first” responsive
design aren’t relevant any longer — the new way of building layouts approaches 
design “element-first.” Using element queries enables you to work on the 
individual parts that make up a layout, in isolation from one another, styling 
them to a greater level of detail. If you are using a modular approach for your 
back-end code already but have so far been unable to package your CSS with your 
modules because of the difficulties of styling with media queries alone, then 
element queries will finally allow you to modularize your styles in the same way.

### Thinking Element-First {#thinking-element-first}

Element-first design is in the same spirit as the [atomic design principle][39]
[41][40] but looks different in practice from how most people have implemented
atomic design in the past.

For example, let’s say you have some HTML like the following, and the desired
responsive behavior can be explained as, “The search input and button are 
displayed side by side until the form gets too narrow. Then, both the input and 
the button should be stacked on top of each other and displayed full width.
”

    <form>
      <input type=search>
      <input type=button value=Search>
    </form>

#### Desktop-First Approach {#desktop-first-approach}

In a desktop-first mindset, you would write styles for the desktop layout first
and then add responsive support for smaller screens.

    input {
      width: 50%;
      float: left;
    }
    @media (max‐width: 600px) {
      input {
        width: 100%;
        float: none;
      }
    }

#### Mobile-First Approach {#mobile-first-approach}

In a mobile-first mindset, you would design the mobile view first and add
support for the side-by-side view only when the screen is wide enough.

    input {
      width: 100%;
    }
    @media (min‐width: 600px) {
      input {
        width: 50%;
        float: left;
      }
    }

#### Element-First Approach {#element-first-approach}

In the first two examples, the media breakpoint was set to 600 pixels, and not
because that’s how wide the inputs will be when they switch. Chances are, the 
search input is probably inside at least one parent element that would have 
margins or padding. So, when the browser is 600 pixels wide, those inputs might 
be somewhere around 550 or 525 pixels wide on the page. In a desktop-first or 
mobile-first approach, you’re always setting breakpoints based on the layout and
how elements show up within it. With an element-first layout, you’re saying, “I 
don’t care how wide the browser is. I know that the sweet spot for where I want 
the inputs to stack is somewhere around 530 pixels wide.” Instead of using a 
media query to swap that CSS based on the browser’s dimensions, with element-
first design, we would scope our responsive style to the`form` element itself,
writing a style like this:

    input {
      width: 100%
    }
    @element 'form' and (min‐width: 530px) {
      $this input {
        width: 50%;
        float: left;
      }
    }

The code is similar to that of the two previous methods, but now we are free to
display this search input anywhere — in a sidebar or full width. We can use it 
in any layout on any website, and no matter how wide the browser is, when the 
form itself doesn’t have enough room to display the inputs side by side, it will
adapt to look its best.

### Resources For Getting Started {#resources-for-getting-started}

#### EQCSS-Enabled Template {#eqcss-enabled-template}

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf‐8">
      <title></title>
      <style></style>
    </head>
    <body>
    
      <!‐‐[if lt IE 9]><script src="http://elementqueries.com/EQCSS‐polyfills.min.js"></script><![endif]‐‐>
      <script src="http://elementqueries.com/EQCSS.min.js"></script>
    </body>
    </html>

#### Demos {#demos}

#### Further Reading {#further-reading}

You can find the [EQCSS project on GitHub][41][70][42], [demos][7][71][43],
documentation and articles on the[EQCSS website][44][72][45]. An ever-growing
number of[Codepens use EQCSS][46][73][47], and you can create your own pen by
forking the[batteries-included template][48][74][49] that comes hooked up with
EQCSS. You can also[play with the EQCSS tool][50][75][51] that I built to
preview if your EQCSS code is working as expected.

Happy hacking!

*(vf, il, al)*

[↑ Back to top][52] [Tweet it][53][Share on Facebook][54]</article>


 [1]: https://www.smashingmagazine.com/2013/06/media-queries-are-not-the-answer-element-query-polyfill "Media Queries Are Not The Answer: Element Query Polyfill"

 [2]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#1
 [3]: img/eqcss-logo-opt.png ""

 [4]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#2
 [5]: https://github.com/eqcss/eqcss

 [6]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#3
 [7]: https://github.com/eqcss/eqcss#element-query-demos

 [8]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#4

 [9]: https://discourse.wicg.io/t/element-queries/26 "WICG Discourse Forum Element Queries Thread"

 [10]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#5
 [11]: https://github.com/eqcss/eqcss "EQCSS.JS"

 [12]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#6
 [13]: https://tldrlegal.com/license/mit-license "MIT License at TLDR Legal"

 [14]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#7
 [15]: http://elementqueries.com/EQCSS.js "EQCSS.js"

 [16]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#8

 [17]: https://drafts.csswg.org/selectors-4/#relational "Description of Has in CSS Selectors Level 4"

 [18]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#9
 [19]: https://api.jquery.com/has-selector/ "jQuery Has selector"

 [20]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#10
 [21]: http://caniuse.com/#feat=css-has "Can I use browser support for Has"

 [22]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#11
 [23]: http://codepen.io/tomhodgins/pen/xOZGOq "This selector demo"

 [24]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#32
 [25]: http://codepen.io/tomhodgins/pen/VjeLjy "Parent selector demo"

 [26]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#33
 [27]: http://codepen.io/tomhodgins/pen/RRrPRy "Root selector demo"

 [28]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#34
 [29]: http://codepen.io/tomhodgins/pen/gMPpMd "Prev selector demo"

 [30]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#35
 [31]: http://codepen.io/tomhodgins/pen/PzZqzy "Next selector demo"

 [32]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#36

 [33]: https://blogs.msdn.microsoft.com/ie/2008/10/16/ending-expressions/ "MSDN Blogs Ending Expressions"

 [34]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#38

 [35]: https://www.smashingmagazine.com/2007/07/css-specificity-things-you-should-know/ "CSS specificity things you should know"

 [36]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#39

 [37]: https://www.smashingmagazine.com/2016/03/houdini-maybe-the-most-exciting-development-in-css-youve-never-heard-of/ "Houdini may be the most exciting development in CSS youve never heard of"

 [38]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#40
 [39]: http://bradfrost.com/blog/post/atomic-web-design/ "Atomic web design"

 [40]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#41
 [41]: https://github.com/eqcss/eqcss "EQCSS on Github"

 [42]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#70

 [43]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#71
 [44]: http://elementqueries.com "element queries dot com"

 [45]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#72

 [46]: http://codepen.io/search/pens/?q=eqcss&limit=all&order=newest&depth=everything&show_forks=true "Pens on Codepen using EQCSS"

 [47]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#73

 [48]: http://codepen.io/pen?template=gagyrz "EQCSS batteries included template"

 [49]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#74
 [50]: http://elementqueries.com/repl.html

 [51]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/#75

 [52]: https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/

 [53]: https://twitter.com/intent/tweet?original_referer=https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/&source=tweetbutton&text=The%20Search%20For%20The%20Holy%20Grail%3A%20How%20I%20Ended%20Up%20With%20Element%20Queries%2C%20And%20How%20You%20Can%20Use%20Them%20Today&url=https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/&via=smashingmag

 [54]: http://www.facebook.com/sharer/sharer.php?u=https://www.smashingmagazine.com/2016/07/how-i-ended-up-with-element-queries-and-how-you-can-use-them-today/