# The mt proteome from Morgenstein et al.

- Last modified: tis aug 09, 2022  06:29
- Sign: JN

## Description

**Task**: Retrieve the proteome described/used in [Morgenstern et al.
2021](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8664129/)

Supplemental information indicates that data are available and details can be
found in "Key resources table":
<https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8664129/#sec4.1>

More specifically, the section called Deposited Data looks promising:

```
Proteomic datasets (MS/MS raw files and MaxQuant analysis files)	This paper
ProteomeXchange: PXD016924, PXD018122, PXD018182, PXD028149, PXD028169,
PXD029242
```

The ID's starting in `PXD` are dataset IDs on the [ProteomeExchange
site](http://www.proteomexchange.org/).  Each dataset have a description page
than can be accessed by, e.g.,
<http://proteomecentral.proteomexchange.org/cgi/GetDataset?ID=PXD016924>

On these pages, we can see that the access to data can be made by [anonymous
ftp](https://www.cs.colostate.edu/helpdocs/ftp.html).  Then, in principle, one
can use the command `ftp ftp.pride.ebi.ac.uk` to connect to the ftp-server.
Use `anonymous` as user, and your email as password, when prompt for.  After
successful login, one may change directory by using, e.g., `cd
pride/data/archive/2021/11/PXD016924`, and then list the content of the current
working directory using the command `dir`. This will display the files and we
can then download the fasta file using `get
UniProt_ProteomeSet_Human_Isoform.fasta`.  The connection can then be closed
using the command `bye`. This has to be repeated for all IDs.

One alternative, using `curl`, is outlined below.

For the six IDs, we can get the links by reading the individual
proteomexchange.org websites and look for a ftp link like so:

    $ for id in PXD016924 PXD018122 PXD018182 PXD028149 PXD028169 PXD029242 ; do
        curl -v --stderr - \
        "http://proteomecentral.proteomexchange.org/cgi/GetDataset?ID=${id}" | \
        grep -m 1 'ftp://' | \
        awk '{print $2}' | \
        sed 's/href=//'
      done

    ftp://ftp.pride.ebi.ac.uk/pride/data/archive/2021/11/PXD016924
    ftp://ftp.pride.ebi.ac.uk/pride/data/archive/2021/11/PXD018122
    ftp://ftp.pride.ebi.ac.uk/pride/data/archive/2021/11/PXD018182
    ftp://ftp.pride.ebi.ac.uk/pride/data/archive/2021/11/PXD028149
    ftp://ftp.pride.ebi.ac.uk/pride/data/archive/2021/11/PXD028169
    ftp://ftp.pride.ebi.ac.uk/pride/data/archive/2021/11/PXD029242

We can see that all data are located in the same folder on the ftp server
(`pride/data/archive/2021/11`)

We wish to download any sequence data in the PXD folders, preferably in fasta
format.  At this stage, however, we don't know the name of the fasta file.

From checking manually (using `ftp`) the content of the first folder, we assume
that all fasta files ends in `.fasta`.  We can then try `curl` to get the file
names in each subfolder, and then use that name in a second call to `curl` to
download (a total of 278M) :

    $ for id in PXD016924 PXD018122 PXD018182 PXD028149 PXD028169 PXD029242 ; do
        fn=$(curl -v --stderr - \
        "ftp://anonymous:''@ftp.pride.ebi.ac.uk/pride/data/archive/2021/11/${id}/" | \
        grep 'fasta' | \
        awk '{print $NF}')
        curl \
        "ftp://anonymous:''@ftp.pride.ebi.ac.uk/pride/data/archive/2021/11/${id}/${fn}" \
        -o ${id}.fasta
      done

### Reference

- Morgenstern, M. et al. 2021. Quantitative high-confidence human mitochondrial
  proteome and its dynamics in cellular context.  Cell Metab.
  33(12):2464-2483.e18. doi: 10.1016/j.cmet.2021.11.001

