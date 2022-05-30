# Setup on macos

- Last modified: mån maj 30, 2022  05:53
- Sign: JN

## Local bin and path

    $ mkdir -p /Users/$USER/bin

Create/edit the `~/.bash_profile` (`/Users/$USER/.bash_profile`) to include

    if [ -d /Users/$USER/bin ] ; then
        export PATH=/Users/$USER/bin:$PATH
    fi

## Install [fasta-tab](https://github.com/nylander/fasta-tab)

    $ cd /Users/$USER/bin
    $ curl -o fasta2tab "https://raw.githubusercontent.com/nylander/fasta-tab/master/fasta2tab"
    $ curl -o tab2fasta "https://raw.githubusercontent.com/nylander/fasta-tab/master/tab2fasta"
    $ chmod +x fasta2tab tab2fasta

## Install [grepfasta](https://github.com/nylander/grepfasta)

    $ cd /Users/$USER/bin
    $ curl -o grepfasta.pl "https://raw.githubusercontent.com/nylander/grepfasta/master/grepfasta.pl"
    $ chmod +x grepfasta.pl

## Install [translate_fasta_headers](https://github.com/nylander/translate_fasta_headers)

    $ cd /Users/$USE/bin
    $ curl -o translate_fasta_headers.pl \
        "https://raw.githubusercontent.com/nylander/translate_fasta_headers/master/translate_fasta_headers.pl"
    $ chmod +x translate_fasta_headers.pl
    $ curl -o replace_taxon_labels_in_newick.pl \
        "https://raw.githubusercontent.com/nylander/translate_fasta_headers/master/replace_taxon_labels_in_newick.pl"
    $ chmod +x replace_taxon_labels_in_newick.pl

(See <https://perldoc.perl.org/perlmacosx> for Perl-issues)

## Install [blast+](https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/)

From <https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/>

- Download the `macosx.tar.gz` and `macosx.tar.gz.md5` (or the `*.dmg.*` ?)
- Check md5 checksum (compare with the file`macosx.tar.gz.md5`): `md5 macosx.tar.gz`
- Proceed with installation:
    - <https://www.ncbi.nlm.nih.gov/books/NBK569861/#intro_Installation.MacOSX>
    - <https://www.ncbi.nlm.nih.gov/books/NBK52640/>

## Install git

<https://www.atlassian.com/git/tutorials/install-git>

<https://git-scm.com/download/mac>

<https://sourceforge.net/projects/git-osx-installer/>

Which version should I download?

If you are running:

- `10.6` Snow Leopard: git-*-snow-leopard
- `10.7` Lion: git-*-snow-leopard
- `10.8` Mountain Lion: git-*-snow-leopard
- `10.9` Mavericks: git-*-mavericks
- `10.10` Yosemite: git-*-mavericks
- `10.11` Yosemite: git-*-mavericks


