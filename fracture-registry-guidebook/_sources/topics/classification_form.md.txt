# The classification dialog

The classification dialog is perhaps the most complicated part of the fracture
registry. In short, it's the dialog that we display after the user clicks on the
[skeleton selector](skeleton_selector.md). It lets the user select among the
various fractures that are relevant to the segment they clicked on the skeleton.

The reason the classification dialog is complex is because of how it gets
configured. For reasons of maintainability, we decided to design it so that it
could be given different "specifications" that determined what it would display.
So we have a core implementation that doesn't know anything about particular
classifications for bone segments. We then instantiate the dialog with
parameters that tell it what fractures to show and how to lay them out. The core
implementation examines these specifications and draws the right dialog.

## Fracture classification specifications

The file `fracture-classification-specs.js` defines two factories,
`AdultFractureClassificationSpecs` and `PediatricClassificationSpecs`, which
contain the specifications for all of the different fracture dialogs. These
objects are dictionaries that map
from [bone segment prefixes](fracture_codes.md) to *classification specs*. Each
classification spec defines the particular set of fractures and dialog layout
for that bone segment.

## Fracture descriptions

Similar to the classifications specifications, the fracture classification
dialog needs textual descriptions of fractures. It gets these from the
`fractureDescriptions` service. These are loaded from the server in the same
manner as the classification specs.

## Views

The final leg of the fracture classification dialog is the set of view templates
that comprise its UI. There are three main templates to consider:

1. `fractureClassificationDialog.html` - this is the top-level template for the
   entire dialog.
2. `fractureClassificationBoneFrame` - this represents the classification
   options for a single bone in the fracture. If the fracture includes only one
   bone, then this is displayed "bare". If there are multiple bones in the
   fracture, there are multiple of these frames and each is displayed as a tab.
3. `fractureClassificationOption` - This displays a single classification option
   within a `fractureClassificationBoneFrame`.

These rely on other directives and template (e.g. `fracture-image`) to do their
work. At its core, the fracture dialog works by iterating over a classification
specification and creating the right set of bone frames and classification
options. The reason the dialog feels complex is because it's an abstraction; it
only makes complete sense when all of its parts are combined.

## Putting it all together

While the classification dialog is fairly complex, I don't want to reiterate too
much of its internals here. Instead, I hope the code can speak for itself. If
you can keep in mind what it's trying to do --- that is, provide an extensible
system for displaying fracture classification options --- then I think it's
relatively clear.
