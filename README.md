# readytowear
Ready-made Taxonomic Weights Repository

Ready-made taxonomic weights generated by [q2-clawback](https://github.com/BenKaehler/q2-clawback) for use with the [q2-feature-classifier](https://github.com/qiime2/q2-feature-classifier/) taxonomy classifier.

If you use any materials in readytowear, please cite:

Benjamin D Kaehler, Nicholas Bokulich, Daniel McDonald, Rob Knight, J Gregory Caporaso, Gavin A Huttley. Species abundance information improves sequence taxonomy classification accuracy. bioRxiv 406611; doi: https://doi.org/10.1101/406611


## How to use the readytowear collection

**NOTE:** *The readytowear collection currently only includes taxonomic weights generated for 16S rRNA gene sequence data using the Greengenes reference database trimmed to the V4 domain using primers 515f and 806r. Hence, the collection currently does not include weights for other marker genes or other 16S rRNA gene domains. We will accommodate these others needs in future releases, and encourage community contributions (contribution instructions coming soon). In the mean time, if you use non-V4 or non-16S rRNA gene data and wish to use bespoke classifiers, assemble your own custom taxonomic weights with q2-clawback as described [here](https://forum.qiime2.org/t/using-q2-clawback-to-assemble-taxonomic-weights/5859)*

q2-feature-classifier is a plugin for [QIIME 2](https://qiime2.org/), and hence QIIME 2 must be installed to use. Before beginning this tutorial, install and activate your QIIME 2 environment.

Clone readytowear to get started:
```
git clone https://github.com/BenKaehler/readytowear.git
```

Train a non-saline soil naive Bayes taxonomy classifier using the latest readytowear fashions:
```
qiime feature-classifier fit-classifier-naive-bayes \
  --i-reference-reads readytowear/data/gg_13_8/515f-806r/ref-seqs-v4.qza \
  --i-reference-taxonomy readytowear/data/gg_13_8/515f-806r/ref-tax.qza \
  --i-class-weight readytowear/data/gg_13_8/515f-806r/soil-non-saline.qza \
  --o-classifier gg138_v4_soil-non-saline_classifier.qza
```

Now this classifier is ready to use! Classify a set of query sequences contained in a FASTA format file as follows:
```
qiime tools import \
  --input-path sequences.fna \
  --output-path sequences.qza \
  --type 'FeatureData[Sequence]'

qiime feature-classifier classify-sklearn \
  --i-reads sequences.qza \
  --i-classifier gg138_v4_soil-non-saline_classifier.qza \
  --o-classification bespoke-classifier-results.qza

  qiime metadata tabulate \
    --m-input-file bespoke-classifier-results.qza \
    --m-input-file sequences.qza \
    --o-visualization bespoke-classifier-results.qzv
```
