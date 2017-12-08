# Data

There's very little surprising about the data in Fracreg. The form data is
stored using EntityFramework, just like any other MRS registry.

There are a few places where Fracreg relies on some fairly special input data.
Specifically:

 * We specify a lot of fracture classification details in a spreadsheet which we
 import into the registry as CSV.
 * We include NCSP and ICD-10 data in the registry via CSV files as well.

We cover the details of this data and associated procedures in other parts of
this documentation.
