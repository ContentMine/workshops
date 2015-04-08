![ContentMine logo](https://github.com/ContentMine/assets/blob/master/png/Content_mine(small).png)

# Fact extraction with AMI

AMI is a suite of software that allows the extraction of facts from text. It is run from the command line, and uses a plugin system so that each kind of fact has its own plugin. For example, if you want to find all mentions of biological species, you would use `ami` with the `species` plugin.

This directory contains materials for tutorials and demonstrations of the fact extraction pipeline, using `ami` with various plugins (`species`, `regex`, `identifier`, `sequence`, etc.).

## Contents

- [Getting AMI](getting_ami.md)
- [AMI basics](ami_basics.md)
- Plugins:
  - [ami-words](ami_words.md) performs simple word-counting and overrepresentation analysis
  - [ami-species](ami_species.md) finds mentions of biological species names
  - [ami-sequence](ami_sequence.md) finds nucleotide (DNA/RNA) and peptide (protein) sequences
  - [ami-identifier](ami_identifier) finds structured resource identifiers (e.g. DOIs, PMIDs, GitHub URLs, Data Dryad IDs)
  - [ami-regex](ami_regex.md) finds entities matched by user-specified patterns
