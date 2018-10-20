Taxonomy
--------
This is a Rust library for reading, writing, and editing biological taxonomies. There are associated Python bindings for accessing most of the functionality from Python.


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
