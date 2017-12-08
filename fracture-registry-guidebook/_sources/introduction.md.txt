# Introduction

The Norwegian Fracture Registry (a.k.a Fracreg) is a registry for capturing
information about the circumstances, nature, treatment, and outcomes of bone
fractures. It primarily comprises a web-based client for entering and querying
data coupled with a web server and database for storing and managing the data.
Fracreg isn't hugely complex or terribly sophisticated, though - as with most
software systems - there are parts of it that are substantially more complex
than others.

This guidebook is designed to orient software developers who need to understand
how Fracreg works. In general, I'll assume that readers are familiar with the
core technologies used in the implementation, e.g. C#, JavaScript, Angular, MVC,
etc. I'll also try to avoid spending too much time describing aspects of the
system where the code "speaks for itself". Instead, I'll focus on architectural
aspects of the system, places where the code may be obscure or complex, or
generally places where I think some documentation will prevent significant
confusion on the part of developers.

## MRS

Fracreg is based upon the MRS registry framework, and MRS defines a lot of how
the registry is built and operates. I'm going to assume that readers have a
reasonable understanding of MRS, and I'll try to avoid covering topics here that
are better covered by MRS's own documentation.

## The "guidebook" approach

The idea of a "software guidebook" is based on the idea of a travel guidebook.
Travel guidebooks orient travelers with information at different levels of
detail: country, region, city, neighborhood, and even individual businesses and
places of interest. This lets readers quickly get the information they need at
the appropriate "zoom level".

A software guidebook like this tries to serve the same purpose. I'll provide
descriptions of Fracreg at different levels of detail so that different
users/stakeholders can find the information they need quickly and accurately.
This guidebook will point readers in the right direction and perhaps explain
"exotic local customs", but as much as possible it will let the code speak for
itself.

This approach to software documentation comes from Simon Brown's
["Software Architecture for Developers" books and courses](https://leanpub.com/software-architecture-for-developers)
which I highly recommend.
