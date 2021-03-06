Ontology Scripts
======

These are some general-purpose utility scripts that can be used to 
convert trait ontologies between various different formats that are 
used internally to manage the traits, by the Crop Ontology website, 
and by the breeDBase backend.

File Formats
-----

**Trait Workbook:** An Excel workbook containing separate worksheets for 
each data type: Variables, Traits, Methods, Scales, Trait Classes, and 
Ontology Root Information.  This file is used internally to edit and/or 
add new traits.

**Trait Dictionary:** A semi-colon separated file used by the Crop 
Ontology website.  Each row of a Trait Dictionary is an observation 
variable and contains the complete trait, method and scale information 
for the variable.

**OBO:** A text file format used by OBO-Edit to store ontology information.
Each data type (Variable, Trait, Method, Scale) is represented by a 
`[Term]` block and their relationships are defined by `is_a`, `method_of`, 
`scale_of` and `variable_of` terms.

  * The standard-obo file contains separate namespaces for each data type
    
  * The sgn-obo file uses a single namespace for the root elements, variables 
    and traits
    
    
Scripts
-----

**create_tw.pl** - Create a Trait Workbook from an existing Trait Dictionary

```
NAME
    create_tw.pl

SYNOPSIS
    Usage: perl create_tw.pl -d namespace -n name -r root -o output [-v]
    input

    Options/Arguments:

    -d      the default ontology namespace. This will be used when
            generating an obo file (ex sugar_kelp_trait).

    -n      the ontology display name. A human-readable name for the
            ontology (ex Sugar Kelp Traits).

    -r      the ontology root id. Most likely the Crop Ontology ID (ex
            CO_360).

    -o      the output location of the trait workbook excel file (xlsx
            extension).

    -v      verbose output

    input   specify the Crop Ontology Root ID (ex: CO_360) to download the
            trait dictionary from cropontology.org OR the file path to an
            existing trait dictionary.

DESCRIPTION
    This will create a 'Trait Workbook' Excel file from an existing Crop
    Ontology 'Trait Dictionary'. The Trait Dictionary can be specified by
    it's CO ID (such as CO_360) and downloaded from the Crop Ontology
    website OR by a file path to an existing Trait Dictionary file.

    The resulting Trait Workbook will contain the worksheets 'Variables',
    'Traits', 'Methods', 'Scales', 'Trait Classes' and 'Root'. Some columns
    will have conditional formatting applied that will highlight duplicated
    values. The 'Trait name', 'Method name' and 'Scale name' columns in the
    'Variables' worksheet will highlight names of elements that do not match
    existing elements.

    The Trait Workbook file can be used by the build_traits.pl script to
    build a Trait Dictionary and/or OBO file.

AUTHOR
    David Waring <djw64@cornell.edu>
```

**build_traits.pl** - Build a Trait Dictionary and/or Standard-OBO file from a Trait Workbook

```
NAME
    build_traits.pl

SYNOPSIS
    Usage: perl build_traits.pl [-o output -u username] [-t output] [-i
    institution] [-fv] file

    Options/Arguments:

    -o      specify the output location for the generic obo file

    -u      specify the username of the person generating the file(s)
            required when generating an obo file

    -t      specify the output location for the trait dictionary file

    -i      filter the output to contain only the variables used by the
            specified institution

    -f      force the generation of the files (ignore the unique and
            required checks)

    -v      verbose output

    file    file path to the trait workbook

DESCRIPTION
    Build a trait dictionary and/or standard obo file from a "trait
    workbook" (an Excel workbook containing worksheets for a trait
    ontology's "Variables", "Traits", "Methods", "Scales", "Trait Classes"
    and "Root" information).

AUTHOR
    David Waring <djw64@cornell.edu>
```

**convert_obo.pl** - Convert a Standard-OBO file to an SGN-OBO file

```
NAME
    convert_obo.pl

SYNOPSIS
    Usage: perl convert_obo.pl -d namespace -n namespace -o output [-v]
    input

    Example: perl convert_obo.pl -d sugar_kelp_traits -n sugar_kelp_traits
    -n sugar_kelp_variables -o sgn.obo standard.obo

    Options/Arguments:

    --default, -d
            the default namespace to be used in the sgn-obo file

    --namespace, -n
            The namespace(s) from the standard-obo file to keep in the
            sgn-obo file. These will be renamed to the default namespace
            (-d). This option can be used more than once to specify multiple
            namespaces to include.

            This should include the namespaces of the ontology root term,
            trait classes, traits, and variables.

            If the standard-obo file was generated using the build.pl
            script, this will likely include the namespaces of {default
            namespace}, {default namespace}_trait and
            {default_namespace}_variable.

    --output, -o
            specify the output location for the sgn-obo file

    --verbose, -v
            verbose output

    input   specify the location of the input standard-obo file

DESCRIPTION
    Convert a standard-obo file into an sgn-obo file to be loaded into a
    breeDBase instance.

    This will rename all of the specified standard-obo namespaces into the
    single default namespace.

AUTHOR
    David Waring <djw64@cornell.edu>
```

### Perl Dependencies

- Excel::Writer::XLSX
- Excel::Writer::XLSX::Utility
- Spreadsheet::Read
- DateTime::Format::Excel