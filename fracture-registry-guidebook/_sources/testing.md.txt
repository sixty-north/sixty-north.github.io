# Testing

Fracreg has a suite of tests that we use to ensure that it behaves properly. At
the time of this writing the tests are incomplete (and in fact are less complete
than they used to be...more on that later), but they work well as a smoke test
for regressions when making changes.

## Low-level unit tests

There are a number of fairly low-level unit tests in Client.Web.UI/tests. These
were initially developed to test complex parts of the javascript codebase, but
they've largely been superceded by higher-level behavioral tests.

Most of theses tests still work, though some are currently failing. We've kept
this infrastructure around in case we decide to write some more tests. The test
system we've got serves as a good record of *how* to set up these kinds of
tests, so it could be handy.

These tests have a good README that describes how to run them. Details on what
technologies are used, etc. should be apparent from the code itself.

## High-level behavioral tests

The more important tests for Fracreg are the high-level, BDD-style tests. These
test the full registry software stack, driving the system via a browser much
like a user would. In principle we could use these tests to verify (nearly)
every behavior of the system, though the tests currently only cover a small
portion of registry's functionality.

For historical reasons, these tests are kept in a separate git
repository:
[github.com/abingham/fraktur-register-tests](https://github.com/abingham/fraktur-register-tests)

Obviously we should probably move these tests somewhere more appropriate, and
there is an open basecamp TODO for this.
