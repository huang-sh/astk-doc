# Quick start

## introduction

In this tutorial, we introduce how to utilize **ASTK** to rapidly perform differential alternative splicing analysis using mouse forebrain RNA-Seq data from seven embryonic stages(e11.5–e16.5, p0).

## data preparation

**ASTK** requires transcript TPM(transcript per million) quantification files as input for differential splicing analysis. We have provided [quantification data](https://raw.githubusercontent.com/huang-sh/astk/main/demo/data.tar.gz) for quick start. However, you can still download raw data from [ENCODE](https://www.encodeproject.org/search/?type=Experiment&control_type!=*&assay_term_name=polyA%20plus%20RNA-seq&replicates.library.biosample.donor.organism.scientific_name=Mus%20musculus&biosample_ontology.term_name=forebrain&status=released) and refer to [Appendix](https://huang-sh.github.io/astk-doc/#/en/content/appendix) for [transcript quantification](https://huang-sh.github.io/astk-doc/#/en/content/appendix?id=transcript-quantification).

[Genome annotation](https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_mouse/release_M25/gencode.vM25.annotation.gtf.gz) (GENCODE, mouse, release_M25) is also required for alternative splicing event inference.

[Genome fasta](https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_mouse/release_M25/gencode.vM25.annotation.gtf.gz) (GENCODE, mouse, release_M25) is also required for subsequent analyses, but not in the quick start tutorial.

The following is the quantification data:

```bash
$ ll  data/quant | cut -d " " -f 5-
146 Oct 30  2021 fb_e11.5_rep1
146 Oct 30  2021 fb_e11.5_rep2
146 Oct 30  2021 fb_e12.5_rep1
146 Oct 30  2021 fb_e12.5_rep2
146 Oct 30  2021 fb_e13.5_rep1
146 Oct 30  2021 fb_e13.5_rep2
146 Oct 30  2021 fb_e14.5_rep1
146 Oct 30  2021 fb_e14.5_rep2
146 Oct 30  2021 fb_e15.5_rep1
146 Oct 30  2021 fb_e15.5_rep2
146 Oct 30  2021 fb_e16.5_rep1
146 Oct 30  2021 fb_e16.5_rep2
146 Oct 30  2021 fb_p0_rep1
146 Oct 30  2021 fb_p0_rep2

$ ll data/quant/fb_e11.5_rep*/quant.sf | cut -d " " -f 5-
6970134 Aug 29 15:23 data/quant/fb_e11.5_rep1/quant.sf
6981746 Aug 29 15:23 data/quant/fb_e11.5_rep2/quant.sf
```

## Metadata

**meta** is used to generate a metadata table for pairwise comparison of multiple groups.

Arguments:

- -o: output path
- -repN: replicate number
- -c1: condition 1(ctrl) sample transcript quantification files
- -c2: condition 2(case) sample transcript quantification files
- -gn: group names

### example 1

If we want to perform AS differential splicing analysis with fb_e11.5 as the control group and other stage data as the treatment group, we can run the command like:

```bash
$ mkdir metadata -p
$ astk meta -o metadata/fb_e11_based -repN 2 \
    -c1 data/quant/fb_e11.5_rep*/quant.sf \
    -c2 data/quant/fb_e1[2-6].5_rep*/quant.sf  data/quant/fb_p0_rep*/quant.sf \
    -gn fb_e11_12 fb_e11_13 fb_e11_14 fb_e11_15 fb_e11_16 fb_e11_p0

```

The output are a CSV file and a JSON file, where the CSV file is easy to view and the JSON file is used for subsequent analysis.

<img src='static/img/fb_e11.png'></img>

### example 2

If we want to perform AS differential splicing analysis with adjacent stages as the control group and the treatment group, we can run the command like:

```bash
$ mkdir metadata -p
$ astk meta -o metadata/fb_adj_based -repN 2 \
    -c1 data/quant/fb_e1[1-6].5_rep*/quant.sf \
    -c2 data/quant/fb_e1[2-6].5_rep*/quant.sf  data/quant/fb_p0_rep*/quant.sf \
    -gn fb_e11_12 fb_e12_13 fb_e13_14 fb_e14_15 fb_e15_16 fb_e16_p0

```

<img src='static/img/fb_adj.png'></img>

## dsflow

**dsflow** is wrapper of AS events inferring, PSI calculation,  differential splicing analysis and significants differential result selection.

Arguments：

- -od: output dirctory
- -md: metadata json, the output of **meta**
- -gtf: genome annotation GTF file
- -et: AS event type [ALL|SE|A5|A3|MX|RI|AF|AL]
- -m: the method used to differential splicing computation [empirical|classical]
- -p：p-value threshold
- -adpsi: absolute dPSI threshold

code：

```bash
$ mkdir result
$ astk dsflow -od result/fb_e11_based -md metadata/fb_e11_based.json \
    -gtf gencode.vM25.annotation.gtf -et ALL &

$ ls result/fb_e11_based
dpsi  psi  ref  sig01  tpm
```

output contains 4 directories:

- `ref` is used to save AS events references files
- `tpm` is used to save transcript TPM files
- `psi` is used to save AS event PSI files
- `dpsi` is used to save AS event dPSI files
- `sig01` is used to save significants differential result
