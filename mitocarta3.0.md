# MitoCarta 3.0

- Last modified: ons okt 26, 2022  11:48
- Sign: nylander

## Description

MitoCarta3.0: An inventory of mammalian mitochondrial proteins and pathways

Link:

<https://www.broadinstitute.org/mitocarta/mitocarta30-inventory-mammalian-mitochondrial-proteins-and-pathways>


## Human MitoCarta3.0 sequences in fasta format

Download

    $ curl \
      "https://personal.broadinstitute.org/scalvo/MitoCarta3.0/Human.MitoCarta3.0.fasta" \
      -o Human.MitoCarta3.0.fasta

Look at first 10 lines

    $ head Human.MitoCarta3.0.fasta

Extract the first accession number and look at the information in Genbank:

Visit this link: <https://www.ncbi.nlm.nih.gov/search/all/?term=YP_003024037>

## Human MitoCarta3.0 sequences in [BED format](https://genome.ucsc.edu/FAQ/FAQformat.html#format1)

    $ curl \
      "https://personal.broadinstitute.org/scalvo/MitoCarta3.0/Human.MitoCarta3.0.bed" \
      -o Human.MitoCarta3.0.bed

### Display using [UCSC Genome browser](https://genome.ucsc.edu/cgi-bin/hgGateway)

1. Visit [https://genome.ucsc.edu/cgi-bin/hgGateway](https://genome.ucsc.edu/cgi-bin/hgGateway)
2. Choose:
    - Human Assembly -> Feb. 2009 (GRCh37/hg19) -> GO
    - add custom tracks -> Choose file -> Human.MitoCarta3.0.bed -> Submit -> go
3. Search for, e.g., `POLRMT`


### Display using [IGV - Integrative Genomics Viewer](https://igv.org/)

1. Visit [https://igv.org/app](https://igv.org/app)
2. Choose:
    - Genome -> Human (GRCh37/hg19)
    - Tracks -> Local File... -> Human.MitoCarta3.0.bed -> Open
3. Search for, e.g., `POLRMT`

