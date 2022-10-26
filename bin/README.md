# Setup on macos

- Last modified: ons okt 26, 2022  11:48
- Sign: nylander


## What chip is in my Mac?

Go to the Apple Logo > About This Mac

## Install brew, then git

<https://github.com/git-guides/install-git>,
<https://www.freecodecamp.org/news/setup-git-on-mac/>,
<https://brew.sh/>,
<https://techpp.com/2022/05/12/install-git-on-mac-guide/>,

Install homebrew

    $ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

When it prompts you for the admin password, enter it to continue the
installation. If you’re on a non-M1 Mac, running the above command will
automatically set the PATH variable on your Mac, whereas if you’re using an M1
Mac, you’ll need to run the following command to modify the PATH before you can
use Homebrew:

    $ export PATH=/opt/homebrew/bin:$PATH

Once Homebrew is installed, update it and its packages with:

    $ brew update && brew upgrade

And then, install Git by running:

    $ brew install git

Verify the installation using:

    $ git --version

Configure git

    $ git config --global user.name "olawanlejoel"
    $ git config --global user.email "mymail@gmail.com"

## Install git (Xcode)

Note, one may need to install Xcode, which probably requires an Apple developer account...

<https://git-scm.com/book/en/v2/Getting-Started-Installing-Git>

"Xcode version: Try:"

    $ git --version

"If you don't have it installed already, it will prompt you to install it."

## Local bin and path

    $ mkdir -p /Users/$USER/bin

Create/edit the `~/.zshrc` (or `/Users/$USER/.zshrc`) to include

    export PATH=/Users/$USER/bin:$PATH"

## Some extra software

**Note:** 2-4 are shipped as a compressed tar archive [`bundle1.tgz`](bundle1.tgz)

### Install [fasta-tab](https://github.com/nylander/fasta-tab)

    $ cd /Users/$USER/bin
    $ curl -o fasta2tab "https://raw.githubusercontent.com/nylander/fasta-tab/master/fasta2tab"
    $ curl -o tab2fasta "https://raw.githubusercontent.com/nylander/fasta-tab/master/tab2fasta"
    $ chmod +x fasta2tab tab2fasta

### Install [grepfasta](https://github.com/nylander/grepfasta)

    $ cd /Users/$USER/bin
    $ curl -o grepfasta.pl "https://raw.githubusercontent.com/nylander/grepfasta/master/grepfasta.pl"
    $ chmod +x grepfasta.pl

### Install [translate_fasta_headers](https://github.com/nylander/translate_fasta_headers)

    $ cd /Users/$USE/bin
    $ curl -o translate_fasta_headers.pl \
        "https://raw.githubusercontent.com/nylander/translate_fasta_headers/master/translate_fasta_headers.pl"
    $ chmod +x translate_fasta_headers.pl
    $ curl -o replace_taxon_labels_in_newick.pl \
        "https://raw.githubusercontent.com/nylander/translate_fasta_headers/master/replace_taxon_labels_in_newick.pl"
    $ chmod +x replace_taxon_labels_in_newick.pl

### Install [blast+](https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/)

From <https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/>

- Download the `macosx.tar.gz` and `macosx.tar.gz.md5` (or the `*.dmg.*` ?)
- Check md5 checksum (compare with the file`macosx.tar.gz.md5`): `md5 macosx.tar.gz`
- Proceed with installation:
    - <https://www.ncbi.nlm.nih.gov/books/NBK569861/#intro_Installation.MacOSX>
    - <https://www.ncbi.nlm.nih.gov/books/NBK52640/>

