# NCSP and ICD10 metadata

Parts of the registry need metadata for the ICD10 and NCSP coding systems.
Primarily this means descriptions of the codes. To make these details available,
we include CSV files containing all of the metadata with the source code. We
obtain these CSV files from the relevant authoritative bodies and don't maintain
them ourselves. Much like the other "imported" data in fracreg, we have MVC
controllers which can read this data and make it available in various forms
through HTTP endpoints.

From time to time (probably once per year) we should look into updating this
information since it does change over time.
