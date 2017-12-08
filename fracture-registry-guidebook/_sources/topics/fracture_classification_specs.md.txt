# Fracture classification specifications

A central feature of Fracreg is the *fracture classification dialog*, the UI for
classifiying fractures. When a user clicks on the skeleton in the fracture form,
they are presented with a dialog box with different fractures along with their
descriptions. This lets users quickly and visually make accurate
classifications, and these classifications are at the heart of the registry.

In order to simplify the creation and maintenance of these dialogs, we decided
to create a declarative description of the dialogs in the form of a
spreadsheet. When this spreadsheet is updated, we can mechanically (though not
yet automatically) import it into the registry where it is used at runtime
to determine what to display on the classification dialogs. This means that
domain experts can do the real work of maintaining the spreadsheet, and their
work can be directly reflected in the registry with no new programming.

Naturally there are a number of "moving parts" involved in making this system
work. First, there is the spreadsheet itself. Currently it is a Google Docs
spreadsheet; we chose this format because it's trivial to share. The name of
this spreadsheet is (currently) "fracture classification definitions", though
this could change, and it comprises several sheets: one each for adult and
pediatric fracture dialog specifications, and one each for mapping fracture
selections to AO and ICD10 codes in adults and children. It's the fracture
dialog specification sheets we're interested in here.

The dialog specs are initially grouped by segment prefixes. These prefixes are
derived from the AO classification system, so e.g. 11 means "proximal humerus".
For each prefix the spreadsheet provides a description of the bone and segment.
It then specifies a number of "rows" which correspond to rows of images on the
dialog. Each row then specifies a number of fracture codes, one for each image
to be displayed, along with short and long descriptions for the fracture code.

In order to import the specifications into the registry we export each sheet as
comma-separated-values (CSV) and store those CSVs in the Visual Studio project,
referenced as resources. We parse the CSV using the
`HelseVest.Frakturregister.DataTool.SpecParser` class, and we expose the parsed
structures from and HTTP endpoint as JSON objects. The web client fetches these
structures.

## AO and ICD10 classification

The spreasheet containing fracture specifications also contains details about
how to map fracture codes to AO classifications and ICD10 classifications.
Essentially, we map groups of fractures codes (since a single fracture may
actually involve multiple bones) to both an AO code and an ICD10 code. We then
import this information into the registry by again exporting the sheets as CSV
and storing these with the source code. We have code to parse the CSV and
provide classification services using the data.
