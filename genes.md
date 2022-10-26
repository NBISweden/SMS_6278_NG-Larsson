# Look at specific genes

- Last modified: ons okt 26, 2022  11:19
- Sign: JN

## Description

Repeat the steps of downloading a number of related sequences,
perform multiple sequence alignment, and produce a tree.

Genes of interest:

- Cox7A2l (a.k.a. SACAF1)
- Cox4iL
- UQCRC1 (EED triplet)

## First example: Cox7A2l

Start at GenBank by searching all databases:

<https://www.ncbi.nlm.nih.gov/search/all/?term=Cox7A2l>

Precomputed
[orthology](https://en.wikipedia.org/wiki/Sequence_homology#Orthology)
[assessments](https://www.ncbi.nlm.nih.gov/kis/info/how-are-orthologs-calculated/):

<https://www.ncbi.nlm.nih.gov/gene/9167/ortholog/?scope=89593&term=COX7A2L>

See also the link ["Genes similar to COX7A2L"](https://www.ncbi.nlm.nih.gov/gene/9167/ortholog/similargenes/)

- Select some Species
- Click Protein alignment -> one sequence per gene -> Align -> Align
- Download -> FASTA Alignment

Rename downloaded file to a better name:

    $ mv ~/Downloads XP*.aln cox7a2l.fas

Make a copy where we have organism as label. This step can be made manually (by
editing the file `cox7a2l.fas` in a text editor), but doing this
programatically may be more convenient. Here is one example (assumes the format
we have after downloading as per above). We need to be in the directory where
we have the input file `cox7a2l.fas`.

    $ perl -ne 'if (/\[organism=([^]]+)/){
                $label=$1;
                $label =~ s/\s+/_/g;
                print ">$label\n";
              } else {
                print;
              }' cox7a2l.fas > cox7a2l.organism.fas


Re-visit the **NEW** (next generation) version of phylogeny.fr:
[https://ngphylogeny.fr/](https://ngphylogeny.fr/)

We do have, however, a ready alignment, so we will skipt that step in the workflow:

- A la Carte
    - Name: (some name)
    - Tools:
        - Alignment Curation -> BMGE
        - Tree Inference -> PhyML+SMS
        - Tree Rendering -> Newick Display



Re-visit phylogeny.fr (see
[download_blast_alignment_trees.md](download_blast_alignment_trees.md)):
<http://www.phylogeny.fr/simple_phylogeny.cgi> **Service unavailable on Thu 1
sep 2022 14:12:09!**

Alternatives:

- [https://ngphylogeny.fr/](https://ngphylogeny.fr/) **Service unavailable ons 26 okt 2022 23:19:35!**
- <https://www.ebi.ac.uk/Tools/phylogeny/simple_phylogeny/> ([Neighbor
  joining](https://en.wikipedia.org/wiki/Neighbor_joining) tree)
- <http://iqtree.cibiv.univie.ac.at/> ([maximum
  likelihood](https://en.wikipedia.org/wiki/Computational_phylogenetics#Maximum_likelihood)
  tree)

Testing [EBI's web page](https://www.ebi.ac.uk/Tools/phylogeny/simple_phylogeny/):

- STEP 1 - Enter your multiple sequence alignment -> Choose file:
  `cox7a2l.organism.fas`
- STEP 2 - Set your Phylogeny options -> (keep all default settings)
- STEP 3 - Submit your job -> Check box "Be notified by email" -> Enter your
  EMAIL and TITLE (name of job, say "cox7a2l")
- Click on "Submit"

Wait for email from sender `support@ebi.ac.uk` with a download link.
Clicking on the URL in the e-mail will display the tree in a web browser.

The tree is, however, not rooted correctly. Best way then is to download the
tree file (click "View Phylogenetic Tree File", and copy-paste to a new file,
say `cox7a2l.organsim.tre`), and upload it to another website where we can
manipulate the tree.  See example in
[download_blast_alignment_trees.md](download_blast_alignment_trees.md)

Note: The tree and alignment can be uploaded to the same web page for
simultaneous viewing.

Testing the [IQ-TREE webserver](http://iqtree.cibiv.univie.ac.at/):

- Input Data -> Browse... -> `cox7a2l.organism.fas`
- Substitution Model Options -> (keep defaults)
- Branch Support Analysis ->  (keep defaults)
- IQ-TREE Search Parameters -> (keep defaults)
- Enter e-mail
- SUBMIT JOB

The job result will be displayed on a new page in the web browser
