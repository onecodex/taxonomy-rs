Features
--------

 - [ ] Add an error enum with a KeyNotFound error?
 - [ ] Include distances in the JSON exports
 - [ ] `all_weighted_paths` and `maximum_weighted_path` impls that use the distances in the taxonomy itself
 - [ ] Command-line tool
 - [ ] ARB import/export (binary format; not sure if this is taxonomic?)



Editable Taxonomy Trait
-----------------------

Taxonomy editing
 - [ ] An `EditableTaxonomy` trait (see below)
 - [ ] An edit method to remove internal nodes and merge their distances out properly
 - [ ] A `merge` method that combines two taxonomies (assuming they have the same root)
 - [ ] `prune_to` and `prune_away` should be able to take a `Taxonomy` trait

To scope more, but it would be cool to have a trait for editing the taxonomy.

```rust
pub trait EditableTaxonomy<'t, T: 't, D: 't>: Taxonomy<'t, T, D>
where
    T: Clone + Display + PartialEq,
    D: PartialOrd + PartialEq,
{
    fn add_node(&'t self, tax_id: T, parent: T) -> Result<()>;
    fn move_node(&'t self, tax_id: T, new_parent: T) -> Result<()>;
    fn remove_node(&'t self, tax_id: T) -> Result<()>;
    // TODO: should this be split up so these two can be edited separately? if one
    // of these isn't set, it will "erase" that field with this setup
    fn edit_info(&'t self, tax_id: T, name: Option<String>, rank: Option<TaxRank>) -> Result<()>;
    // TODO: should "weight" updates be included in the `move_node` and `add_node` above?
    fn edit_weight(&'t self, tax_id: T, weight: D) -> Result<()>;
}
```

The `add_node` impl for this on `GeneralTaxonomy` could look like:
```rust
pub fn add_node(&mut self, tax_id: &str, parent: IntTaxID) -> IntTaxID {
    self.tax_ids.push(tax_id.to_string());
    self.parent_ids.push(parent);
    self.parent_weights.push(1.);
    self.ranks.push(None);
    self.names.push("".to_string());

    let int_tax_id = self.tax_ids.len() - 1;
    if let Some(ref mut lookup) = self.tax_id_lookup {
        lookup.insert(tax_id.to_string(), int_tax_id);
    };
    if let Some(ref mut lookup) = self.children_lookup {
        lookup.push(Vec::new());
        lookup[parent].push(int_tax_id);
    };
    int_tax_id
}
```


Command Line Binary
-------------------

Initial thoughts below:
```rust
#[cfg(feature = "cli")]
#[macro_use]
extern crate clap;

#[cfg(feature = "cli")]
use clap::{App, Arg};

// TODO: it would be nice to have the CLI feature packages specifically for the
// `bin.rs` itself, but that's not supported yet:
// https://github.com/rust-lang/cargo/issues/1982

#[cfg(not(feature = "cli"))]
fn main() {
    panic!("taxonomy needs to be compiled with CLI support")
}

#[cfg(feature = "cli")]
fn main() {

    let matches = App::new("taxonomy")
        .version(crate_version!())
        .author(crate_authors!())
        .about("Manipulate biological taxonomies")
        .setting(AppSettings::VersionlessSubcommands)
        .setting(AppSettings::ArgRequiredElseHelp)
        .arg(Arg::with_name("TAXONOMY")
             .help("The file/directory containing the taxonomy"))
        .get_matches();

    
}

// FIXME
// taxonomy info "Escherichia coli" | 562 -> returns ID 
// taxonomy parent <id> -> returns name/ID of parent
// taxonomy parent --at-rank <rank> <id> -> returns name/ID of parent at that rank
// taxonomy parent --all <id> -> returns name/ID of alls parents (lineage)
//
// taxonomy children <id> -> returns name/ID of children
// taxonomy children --all <id> -> returns name/ID of children, including children of children, etc
//
//
// taxonomy tree --exclude <id>,<id>... --root-at <id>
//
// --tree-file <path> # otherwise try to find one?
// 
// For parent, children, and info
// --output [json|id_and_name|id_only|name_only]  # and rank?
// 
// For tree
// --output [ncbi|newick|json]
```


Available Taxonomies
--------------------

- NCBI
ftp://ftp.ncbi.nih.gov/pub/taxonomy

- Open Tree of Life
https://tree.opentreeoflife.org/about/taxonomy-version/ott3.0

- SILVA
???

- LTP
https://www.arb-silva.de/projects/living-tree/

- GreenGenes (Creative Commons Attribution-ShareAlike 3.0 Unported License)
http://greengenes.secondgenome.com/downloads/database/13_5

- RDP (Creative Commons Attribution-ShareAlike 3.0 Unported License)
http://rdp.cme.msu.edu/misc/rel10info.jsp

- GTDB (Creative Commons Attribution-ShareAlike 4.0 International License)
http://gtdb.ecogenomic.org/downloads
