# Local BLAST

- Last modified: tis maj 31, 2022  03:47
- Sign: Johan.Nylander\@nbis.se

## Task

Run a similarity search using local [BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi),
with one sequence as query, and a mouse proteome as the data base. The result
is a table with similarity scores.

## Download and install blast+

From <https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/>

- Download the `macosx.tar.gz` and `macosx.tar.gz.md5` (or the `*.dmg.*` ?)
- Check md5 checksum (compare with the file`macosx.tar.gz.md5`): `md5 macosx.tar.gz`
- Proceed with installation:
    - <https://www.ncbi.nlm.nih.gov/books/NBK569861/#intro_Installation.MacOSX>
    - <https://www.ncbi.nlm.nih.gov/books/NBK52640/>

## Create a folder for the task

    $ mkdir local_blast
    $ cd local_blast

## Download the proteome from UniProt

Download from this
[*link*](https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/Eukaryota/UP000000589/UP000000589_10090.fasta.gz),
or use the command below

\tiny

    $ curl -o UP000000589_10090.fasta.gz \
    "https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/Eukaryota/UP000000589/UP000000589_10090.fasta.gz"

\normalsize

## Uncompress

    $ gunzip UP000000589_10090.fasta.gz

## Create the database

The selection of sequences will be our data base. For faster access,
blast uses a formatting tool:

    $ makeblastdb -in UP000000589_10090.fasta -dbtype prot

Notice the extra files created in the current working directory.

## BLAST similarity search

### Query sequence

Normally you will already have one or several query sequences. Here, as an
example, we will simply pick one sequence from the database at random and use
as it as our query. We do this by first converting the fasta to tab-delimited
format, then sort the output randomly, then take the first line (by supplying
`-1` as argument to the command `head`), then translate back from tab to fasta
format, and finally saving to file `rand.faa`.  *Note:* this requires the
scripts from
[https://github.com/nylander/fasta-tab](https://github.com/nylander/fasta-tab)
to be installed.  Alternatively, just cut and paste one sequence from the file
`UP000000589_10090.fasta`.

    $ fasta2tab UP000000589_10090.fasta | \
        sort -r | \
        head -1 | \
        tab2fasta > rand.faa

### Similarity search

Run blast with the appropriate algorithm (`blastp` will search an aa-database with an aa-query).

    $ blastp -db UP000000589_10090.fasta -query rand.faa -out blast_output.html -html

View the result (`blast_output.html`) with a web browser.

Normally, you read ("parse") the output with some code for further downstream analyses.
Then having the output as a table (instead of html) is more convenient:

    $ blastp -db UP000000589_10090.fasta -query rand.faa -out blast_output.tab -outfmt 7

View the output with, e.g., a pager (quit by typing the letter `q`):

    $ less -S blast_output.tab

## Extra

Repeat the search, but this time with 10 sequences (sampled randomly from the
data base) instead of one.

