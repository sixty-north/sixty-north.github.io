# Data pre-loading

Where most MRS-based registries load form data on-demand (i.e. as specific form
views are opened), FracReg loads all of its data from the server at once. As
soon as patient is selected, we immediately ask the server for all of the forms
related to that patient. This happens in form-index.js in the `BuildFormIndex`
factory.

This has a few implications for the registry. One is that the initial patient
view can take some time to load. This hasn't been an issue in development, and I
don't anticipate that it will be a problem in production. But it *could* be an
issue, so it's something to keep in mind as we start to use the system at larger
scales.

The second issue is that the client can more easily get out of sync with the
server. If multiple users are working on forms for the same patient, they can
conflict with one another. **There is currently no affordance in the registry
for this.** This could be a problem in practice, and there are a few ways we
could consider fixing it including explicit synchronization (e.g. via
websockets), entity versioning, or form locking.

The reason for pre-loading is that we want users to be able to quickly navigate
between forms (see "Performance" in the ["Quality Attributes" section](../quality_attributes.md).)
