# Fracture codes and AO codes

The AO fracture classification system is a way to identify different kinds of
fractures. One of the major tasks of Fracreg is to be able to assign AO codes to
fractures, so it's useful to know about about the system and how we use it.
Every AO code starts with a two-digit prefix identifying a bone segment, and we
use this prefix fairly pervasively to identify fractures, software elements
related to bones, and so forth. For example, the various clickable ares on the
skeleton selector control each correspond to a specific AO prefix.

While the AO system is quite effective, it isn't quite as precise as we would
like for classifiying all relevant fractures. As a result, we've developed our
own *purely internal* classification system for Fracreg. This system is based
upon (and is, I believe, a proper superset of) the AO system, and when we
classify fractures in Fracreg we are actually assigning these internal codes to
the fractures. Only as a side-effect do we calculate AO classifications for the
fractures.
