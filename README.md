# HATS

Helper for Attribute Template S...... (not a final name)

## Why Three Crates

Why not three crates?

One issue with current procedural macros is that crates defining procedural
macros can't use their own procedural macros. In this case HATS wants to have
some attributes controlling how it parses the attributes (at the moment, only
the top level "scope" attribute identifier, i.e. the value `hats` in
`#[hats(scope = shoes)]`), obviously HATS will need to parse these attributes
out to get the details it needs, but the entire point of HATS is to make parsing
attributes pain-free, having a manual parser in HATS itself mean any upgrades to
the parsing code generated by HATS would probably have to be duplicated in the
parsing code inside HATS itself.

Luckily there is a solution, by using three crates ;-)

The base is `hats-impl`, this implements the entirety of the procedural derive,
but does not itself define a procedural derive macro. Instead it exports a
single function taking in the AST provided to a procedural derive and some
configuration and returns the derived implementation.

Next `hats-bootstrap` defines a simple procedural derive macro
`FromAttributesBootstrap` that calls into `hats-impl` with a hardcoded
configuration.

Finally `hats` defines a slightly more complicated procedural derive macro
(`FromAttributes`) that parses some attributes (using a parser derived with
`FromAttributesBootstrap`), then calls into `hats-impl` with a configuration
derived from those parsed attributes.

So, in the end `hats` is a crate defining a procedural derive macro, to help
developers write procedural derive macros, that uses a procedural derive in its
implementation (no, that is not inception).

## License

Licensed under either of

 * Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you shall be dual licensed as above, without any
additional terms or conditions.
