# Functional overview

As a registry, the primary functionality of Fracreg is fairly well delimited.
Users enter information via a web page, and this data is stored in a database.
Users can then query this data. We do have plans, however, for subsequent phases
of functionality. In this section I'll describe the core functions of Fracreg
and the phases in which they can be conceptually grouped.

## Initial standalone version

The initial version of Fracreg will be much like any other MRS registry, so its
functions will include:

* Gather and store information on incidents, fractures, and procedures
* Allow querying and reports of the stored data
* Support different users with different access rights
* Ensure data integrity, e.g. by requiring certain fields
* Calculate key classification and codes (e.g. NCSP, ICD-10) for the user
* Allow users to view relevant patient history

## Integration with patient journals

To avoid duplicate data entry we want to:

* Pull relevant information from EPJ systems when it's available
* Push relevant information to EPJ systems

## Integration with operation planning systems

We want Fracreg to be able to send information to operation planning systems.
The extent and nature of this data is not well established yet.

## EPROMs

Fracreg will use EPROMs to gather reports from patients.

* Send EPROMs to patients
  * Resending as necessary
  * Reminding patients as necessary
  * Using different modes (email, post, etc.) as necessary
* Receive responses from patients
* Store EPROMs responses in the database
