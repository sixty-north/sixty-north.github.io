# The skeleton selector

The *skeleton selector* is the picture of the skeleton displayed on the fracture
form. When you click on its various bone segments Fracreg displays a dialog for
selecting fractures relevant to that segment. It's designed to make it easy for
users to accurately select fractures, but underneath its friendly exterior lies
a fair amount of complexity that could use a little explanation.

## TL;DR

 * The selector comprises an SVG image and an Angular directive.
 * The top layer of the SVG contains invisible regions corresponding to
   clickable bone segments.
 * The clickable regions are specially labelled so that the `skeleton-selector`
   directive can identify them.
 * The directive attaches click and hover handlers to the clickable regions.
 * The directive manages the *face* (visual settings) of the clickable regions
   based on their current states.
 * The click handlers trigger the classification dialogs, and the results of
   classification are passed to a function which is supplied to the directive.

## The SVG

The SVG skeleton image defines the clickable regions of the selector. There are
two layers of the skeleton that are important. First, there's a bottom layer
that contains the actual visible rendering of the skeleton. Fracreg displays
this layer, but it doesn't interact with or manipulate it in any real way
(except to gray out elements when the selector is disabled).

The second, more interesting layer is the *click layer*, and it sits above the
first. This layer is invisible, and it consists of regions that lie directly
over the various bone segments we've defined on the skeleton. So, for example,
there's an invisible region directly over the left proximal femur and another
over the right humerus shaft. The point of these invisible regions is that we
can programatically attach click handlers to them in the `skeleton-selector`'s
code.

In order to make these regions identifiable in the directive's code, we have
encoded their bone segment details into their IDs. The naming pattern is like
this:

```
fracture-clickable-<AO prefix>-<side>
```

The `AO prefix` part is a two-digit bone-segment prefix from the AO
classification system. The `side` part is one of `left` or `right` to indicate
which side of the body the bone lies on. To continue our example from above,
since the AO prefix for the proximal femur is 31 the left proximal femur's
region has the ID `fracture-clickable-31-left`.

## The directive

Most of the directive is fairly self-explanatory, but a little orientation is in
order. The directive's `link` function does the heavy lifting of scanning the
SVG, locating clickable regions, attaching click and hover handlers, and
otherwise making it interactive.

It first locates all visible elements in the image (technically, all *paths*).
It then also finds those elements which have a click-region ID. It uses the
`data` attributes of the elements to store information about the original face
of the element (opacity, fill, and stroke). Finally, it attaches handlers that:

 * convert region clicks to function calls
 * use hover handlers to update the faces of the clickable regions

This is how the regions turn green/grey/etc. as you interact with them.

The code is reasonably well documented, so hopefully this helps pull it all
together.
