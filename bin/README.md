# Setup on macos

- Last modified: fre jun 10, 2022  01:19
- Sign: JN

## 1. Local bin and path

    $ mkdir -p /Users/$USER/bin

Create/edit the `~/.zshrc` (or `/Users/$USER/.zshrc`) to include

    export PATH=/Users/$USER/bin:$PATH"


**Note:** 2-4 are shipped as a compressed tar archive [`bundle1.tgz`](bundle1.tgz)

## 2. Install [fasta-tab](https://github.com/nylander/fasta-tab)

    $ cd /Users/$USER/bin
    $ curl -o fasta2tab "https://raw.githubusercontent.com/nylander/fasta-tab/master/fasta2tab"
    $ curl -o tab2fasta "https://raw.githubusercontent.com/nylander/fasta-tab/master/tab2fasta"
    $ chmod +x fasta2tab tab2fasta

## 3. Install [grepfasta](https://github.com/nylander/grepfasta)

    $ cd /Users/$USER/bin
    $ curl -o grepfasta.pl "https://raw.githubusercontent.com/nylander/grepfasta/master/grepfasta.pl"
    $ chmod +x grepfasta.pl

## 4. Install [translate_fasta_headers](https://github.com/nylander/translate_fasta_headers)

    $ cd /Users/$USE/bin
    $ curl -o translate_fasta_headers.pl \
        "https://raw.githubusercontent.com/nylander/translate_fasta_headers/master/translate_fasta_headers.pl"
    $ chmod +x translate_fasta_headers.pl
    $ curl -o replace_taxon_labels_in_newick.pl \
        "https://raw.githubusercontent.com/nylander/translate_fasta_headers/master/replace_taxon_labels_in_newick.pl"
    $ chmod +x replace_taxon_labels_in_newick.pl


## 5. Install [blast+](https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/)

From <https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/>

- Download the `macosx.tar.gz` and `macosx.tar.gz.md5` (or the `*.dmg.*` ?)
- Check md5 checksum (compare with the file`macosx.tar.gz.md5`): `md5 macosx.tar.gz`
- Proceed with installation:
    - <https://www.ncbi.nlm.nih.gov/books/NBK569861/#intro_Installation.MacOSX>
    - <https://www.ncbi.nlm.nih.gov/books/NBK52640/>

