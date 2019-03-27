Taxonomy
--------
This is a Rust library for reading, writing, and editing biological taxonomies. There are associated Python bindings for accessing most of the functionality from Python.

This library was developed initially as a component in One Codex's metagenomic classification pipeline before being refactored out, expanded, and open-sourced. It is designed such that it can be used *as is* with a number of taxonomic formats *or* the Taxonomy trait it provides can be used to add last common ancestor, traversal, etc. methods to a downstream package's taxonomy implementation.


Installation
------------

This library can be added to an existing Cargo.toml file and installed straight from crates.io.

To install the Python library on a Mac OS X/Unix system (requires Python 3):
```bash
# you need the nightly version of Rust installed
curl https://sh.rustup.rs -sSf | sh
rustup default nightly

# finally, install the library
./setup.py install  # (or ./setup.py develop)
```


Features
--------

 - [X] NCBI taxonomy format import/export
 - [X] JSON import/export ("tree" and "node_link_data" formats)
 - [X] Newick import/export
 - [X] PhyloXML import
 - [X] Python bindings


Python Usage
------------




Other Taxonomy Libraries
------------------------
There are taxonomic toolkits for other programming languages that offer different features and provided some inspiration for this library:

*ETE Toolkit (http://etetoolkit.org/)* A Python taxonomy library

*Taxize (https://ropensci.github.io/taxize-book/)* An R toolkit for working with taxonomic data
