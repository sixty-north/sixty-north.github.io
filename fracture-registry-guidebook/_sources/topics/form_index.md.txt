# The form index

The `FormIndex` is one of the central data structures in the fracture registry.
It is the "container" for all of the form data that we pull over from the
server. It provides two primary ways of navigating the forms:

1. You can navigate the forms as a tree structure. For example, you can navigate
   from a fracture to its child fracture-bones and procedures.
2. You can query specific types of forms without worrying about the tree
   structure. For example, you could ask for all secondary-fixations without
   reference to their parent forms.

## `EntryBase` and subclasses

Each type of form in the registry --- incidents, fracture, classifications, etc.
--- has an associated "entry" type in the `FormIndex`. An entry instance serves
a few related roles:

1. It contains the forms actual underlying data as fetched from the server.
2. It refers to the repository instance used to communicate with the server
   about forms of that type.
3. It stores any validation errors associated with the form.
4. It monitors changes to the form's data and initiates communication with the
   server to save those changes as necessary.

The `EntryBase` class is the base class for all entry types. It stores the
form's raw data in it's `_data` member, but users should never access this
directly. Instead they should access the `data` member. `data` is given the same
attributes as `_data`, but each is a property that proxies for the associated
attribute on `_data` and tracks when changes occur, triggering saves as
necessary.

Each specific type of form has an `EntryBase` subclass. At a minimum these
subclasses are responsible for initializing `EntryBase` with the correct
repository reference. For example, `IncidentEntry` initializes `EntryBase` with
`Incidentrepository`.

Subclasses are also responsible for constructing the "tree" of forms, though the
way in which this is manages is a bit goofy. For example, `IncidentEntry`
defines an attribute called `fractures` which it initializes to an empty list.
The point of this list is to hold all of the `FractureEntry`s that are children
of the `IncidentEntry`. These child lists are only populated, however, by a
different set of elements in the form-index, the so-called "API"s which we'll
look at soon.

### Subclass use of property accessors

The `data` attribute's properties were initially created only to track attribute
modification and determine when to save a form. However, it turned out the be
convenient to use these properties for enforcing constraints on forms as well.
For example, the `IncidentEntry` overrides the `setAttr` method in order to
ensure that `TransportMode` and `InjuryLocation` are synchronized. Other entries
do similar things.

This seems like a reasonable use of properties and overriding, but I could be
way off base here. If it turns out to be a bad idea, we'll need to find an
alternative.

### Using entries

Entries are used widely throughout the registry code since they hold the form
data. They can be obtained via querying the various form-specific APIs on the
registry (e.g. looking up a fracture by its form ID) or by traversing the tree
(i.e. by iterating through the `fractures` attribute of an `IncidentEntry`).

As mentioned earlier, users should never access the form's data via the `_data`
attribute. They should instead use `data`. They should also never directly
modify the "children" lists such as `IncidentEntry.fractures`. Those should
generally only be modified via the various form factories (discussed later).

## APIs

The public attributes of `FormIndex` are all APIs for accessing the various
types of forms in the registry. So the index has attributes like `fracture`,
`procedure`, and `outpatientBoneProcedure`...one for each form type. Each of
these provides the same set of functions:

- `all` - a list of all forms of that type.
- `add` - add a form to the index.
- `find` - find a form of the associated type by ID.
- `remove` - remove a form from the index by ID.

There are two types of APIs, `RootApi` and `NestedApi`. `RootApi` instances for
for forms which don't have parent forms (in MRS terms, these are non-attached
forms), and they need to interact directly with the `FormIndex._forms` object in
order to store their forms.

`NestedApi` instances are for forms which do have a parent. They are
instantiated with two strings, the name of the form-index attribute under which
they are nested, and the name of their parent "entry" type's attribute to use
for storing forms. For example, the `NestedApi` for fractures is told that it's
nested under `incident` and that it should store its entries on the `fractures`
list of `IncidentEntry`.

This relationship between entries and APIs might seem a bit strained, and
perhaps there's a more elegant way. Things work well enough the way they are
now, though, and I haven't been able to justify the time to really investigate
making things better.

## Factories

While `FormIndex`, entries, and APIs are about storing and querying forms, the
various form factories (form-factories.js) are about creating a deleting form in
a controlled manner. The basic rule is that all form creation and deletion
should be done through the correct factory. So to create a new procedure, use
`ProcedureFormFactory.create()`. Similarly, to delete a consultation, use
`ConsultationFormFactory.delete()`.

A> Note the distinction between "creation" and "instantiation". We instantiate a
A> form when we fetch its data from the server and insert it into the `FormIndex`.
A> There can be, in principle, many instances of a single form at any given time
A> (though we don't do that in the fracture registry). We only *create* a form
A> once. This creates a new ID for the form and establishes that the form exists.
A> This distinction should not cause and problem in practice in the fracture
A> registry, but I want to make clear that factories are for creation and deletion,
A> not instantiation.

Factories know how to coordinate between repositories and form-index. They can
ask the repository (i.e. the server) to create a new form, and then, when the
data for that form arrives, they can insert it correctly into the index.
Critically, they know how coordinate the creation of multiple related forms. For
example, when a procedure is created we need to ensure that it gets a
bone-procedure for each bone in the procedure's parent fracture. The
`ProcedureFormFactory` ensures that this is done properly.

Likewise, factories are responsible for coordinating the deletion of forms. For
example, when we delete an incident we need to delete all of its child forms:
fractures, procedures, etc. The factories take care of this as well (often
relying on transitive deletion by delegating to each other).
