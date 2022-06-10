# Database retrieval, similarity search, multiple sequence alignment, tree inference

- Last modified: fre jun 10, 2022  08:47
- Sign: Johan.Nylander\@bnis.se

## Task

We will use a protein sequence from a [mouse
proteome](https://www.uniprot.org/proteomes/UP000000589), and do similarity
search (using blast), followed by multiple-sequence alignment, and tree
inference.

The exercises are designed to use online tools as much as possible. Some
text editing are required, and for more elaborative tasks, some example tools
are provided (in the [`bin/`](bin/) folder).

## Setup

Open the Terminal application (under `/Applications/Utilities`).

Create a dedicated folder for the session

    $ mkdir session_1
    $ cd session_1

## Download the proteome

Use this
[*link*](https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/Eukaryota/UP000000589/UP000000589_10090.fasta.gz)
or use the command below. The command `curl` ("cat URL") is a browser without a
graphical user interface. Here we use it to download from an URL.

\tiny

    $ curl -o UP000000589_10090.fasta.gz \
    "https://ftp.uniprot.org/pub/databases/uniprot/current_release/knowledgebase/reference_proteomes/Eukaryota/UP000000589/UP000000589_10090.fasta.gz"

\normalsize

Uncompress

    $ gunzip UP000000589_10090.fasta.gz

### Look at the fasta format

    $ less UP000000589_10090.fasta

### Look at the information in UniProt for a fasta record

Copy the accession ID for the first sequence (`A0A075B634`), and use it in the URL for uniprot.org.
Open a web browser on this address:

<https://www.uniprot.org/uniprot/A0A075B634>

For the gene name `Polrmt`, the corresponding ID and address is

<https://www.uniprot.org/uniprot/Q8BKF1>

### Use grep to find the number of sequences

    $ grep '>' UP000000589_10090.fasta
    $ grep -c '>' UP000000589_10090.fasta

### Use grep to find the header of a specific sequence

We will search for the keyword (gene name) `Polrmt`.
The command `grep` will print any line in the infile matching the keyword.

    $ grep 'Polrmt' UP000000589_10090.fasta


---

**tis 31 maj 2022**

---


## Extract one sequence of interest

Here we will use one sequence from the file `UP000000589_10090.fasta` as
example.  We can use dedicated tools for doing so (see below), or just do
cut-and-paste in a text editor.

(Install <https://github.com/nylander/grepfasta>)

    $ grepfasta.pl -p="Polrmt" UP000000589_10090.fasta

The line above prints to the screen. To capture the output and save to a file,
we use redirection (`>`). The redirection operator will create a new file, or
overwrite any existing files with the same name (no warnings!)

    $ grepfasta.pl -p="Polrmt" UP000000589_10090.fasta > Polrmt.faa

The filename `Polrmt.faa` is arbitrary, but we use the gene name, and add
the suffix `.faa` to inform us that it is a fasta-fromatted file, with amino acids.

## Run BLAST (online)

Visit <https://blast.ncbi.nlm.nih.gov/Blast.cgi>

- Check sequence type in our target sequence (nt or aa?).
- Choose [appropriate search
  algorithm](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastp&PAGE_TYPE=BlastSearch&LINK_LOC=blasthome)
- Upload file ("Choose File") `Polrmt.faa`
- Use "Non-redundant protein sequences (nr)" database
- "BLAST"!

### Download the resulting best-hit sequences

Under the tab "Descriptions" and to the right of "Sequences producing
significant alignments", choose Download `FASTA (complete sequence)` (sequence
file named `seqdump.txt`)

Your browser will download a file `seqdump.txt` somewhere on your computer.
On my computer, it ended up in the Downloads folder. Lets move it from there,
and rename it at the same time.

    $ mv ~/Downloads/seqdump.txt blast_out.faa

## Filter sequences

Try filter the results by organism. Let's keep one sequence per species.

Get a list of taxon labels. Here we rely on stacking a number (6) of different
commands after another. The second reads the output from the first, and gives
the ouptut to the third and so on.

    $ grep '>' blast_out.faa | \
        cut -d[ -f 2 | \
        sed 's/]//' | \
        sort | \
        uniq -c | \
        sort -g -r

Save the unique names to a file (`uniq.taxa`)

    $ grep '>' blast_out.faa | \
        cut -d[ -f 2 | \
        sort -u | \
        sed 's/]//' > uniq.taxa

Search the fasta file and extract max one per name

    $ while read -r name ; do
        fasta2tab blast_out.faa | \
        grep "$name" --max-count=1 | \
        tab2fasta
      done < uniq.taxa > blast_out.taxonfilter.faa

How many sequence do we have left?

    $ grep -c '>' blast_out.taxonfilter.faa

## Manipulate (shorten) the sequence labels

    $ translate_fasta_headers.pl blast_out.taxonfilter.faa > short.faa

Notice the translation table generated in the current working directory.
We will edit that file a bit more (we will only keep the taxon labels).

    $ perl -ne \
        'if (/^(Seq_\d+)\t.+\[(.+)\]/){$s=$1;$t=$2;$t=~s/ /_/g;print"$s\t$t\n"}' \
        blast_out.taxonfilter.faa.translation.tab > translation.tab

    $ cat translation.tab

## Do multiple sequence alignment and infer a tree

Phylogeny online <http://www.phylogeny.fr/simple_phylogeny.cgi>

Upload file `short.faa`

When search is finished, download from each tab:

3. Alignment -- download **alignment in Fasta format** (save as
   `short.muscle.faa`)
4. Curation -- download **curated alignment in FASTA format** (save as
   `short.muscle.gblocks.faa`)
5. Phylogeny -- download **Tree in Newick format** (save as
   `short.muscle.gblocks.phyml_wag+g.phy`)

Fix the line breaks in the tree file:

    $ perl -i.bak -pe 's/\n//g' short.muscle.gblocks.phyml_wag+g.phy

Again, the filenames we choose are arbitrary, but here we add information
about how the original file (`short`) was treated: `muscle` from the multiple
sequence alignment, `gblocks` for the alignment filtering, and `phyml_wag+g`
for the tree inference program `phyml`, with the substitution model `WAG+G`.

## Back-translate the short labels to longer

Replace short headers in the sequence file with taxon labels. Save to new file.

    $ translate_fasta_headers.pl \
        -t translation.tab \
        short.muscle.gblocks.faa > long.muscle.gblocks.faa

Replace short headers in the tree file with taxon labels. Save to new file.

    $ replace_taxon_labels_in_newick.pl \
        -t translation.tab \
        short.muscle.gblocks.phyml_wag+g.phy > long.muscle.gblocks.phyml_wag+g.phy

## View the alignment

Visit <https://alignmentviewer.org/> and upload the file `long.muscle.gblocks.faa`.

## View and root the tree

The phylogenetic tree in the previous step was inferred under a non-directional
model of sequence evolution.  In other words, the tree is not rooted. This
means that it does not have a time arrow, and no ancestral-descendant
relationships.

We can root the tree with external information (previous research, e.g.
<http://vertlife.org>): Root the tree between the bats (*Myotis*, *Eptesicus*)
and the rest.

This is easiest done using a dedicated software, for example,
[FigTree](https://github.com/rambaut/figtree/releases), but can also be done in
an online viewer such as IcyTree (<https://icytree.org/>.

In IcyTree, upload the tree file (`long.muscle.gblocks.phyml_wag+g.phy`), then
hover the mouse over the node you want to use as root, then Shift+Left-mouse
click). To save the tree, use `File -> Export tree as -> Newick file`, and save
it as `long.muscle.gblocks.phyml_wag+g.rooted.phy`.

    $ mv ~/Downloads/tree.newick long.muscle.gblocks.phyml_wag+g.rooted.phy

Fix the labels from IcyTree

    $ sed -i 's/"//g' long.muscle.gblocks.phyml_wag+g.rooted.phy

In FigTree, the rerooted tree can be saved (`File -> Export Trees -> Tree file
format:Newick -> OK`) to file name `long.muscle.gblocks.phyml_wag+g.rooted.phy`.

To view the alignment together with the tree, one may also try
<http://www.jalview.org/jalview-js/#>:
- `File -> Input Alignment -> From File` and choose `long.muscle.gblocks.faa`.
- In the alignment window, select `File -> Load Associated Tree` and choose
  `long.muscle.gblocks.phyml_wag+g.rooted.phy` (see below).


