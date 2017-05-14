uom
===
[![Travis](https://travis-ci.org/iliekturtles/uom.svg?branch=master)](https://travis-ci.org/iliekturtles/uom)
[![AppVeyor](https://ci.appveyor.com/api/projects/status/github/iliekturtles/uom?svg=true&branch=master)](https://ci.appveyor.com/project/iliekturtles/uom)
[![Coveralls](https://coveralls.io/repos/github/iliekturtles/uom/badge.svg?branch=master)](https://coveralls.io/github/iliekturtles/uom?branch=master)
[![Crates.io](https://img.shields.io/crates/v/uom.svg)](https://crates.io/crates/uom)
[![Crates.io](https://img.shields.io/crates/l/uom.svg)](https://crates.io/crates/uom)
[![Documentation](https://img.shields.io/badge/documentation-docs.rs-blue.svg)](https://docs.rs/uom)

Units of measurement is a crate that does automatic type-safe zero-cost
[dimensional analysis][analysis]. You can create your own systems or use the pre-built
[International System of Units][si] (SI) which is based on the
[International System of Quantities][isq] (ISQ) and includes numerous [quantities][quantity]
(length, mass, time, ...) with conversion factors for even more numerous
[measurement units][measurement] (meter, kilometer, foot, mile, ...). No more crashing your
[climate orbiter][orbiter]!

[analysis]: https://en.wikipedia.org/wiki/Dimensional_analysis
[si]: http://jcgm.bipm.org/vim/en/1.16.html
[isq]: http://jcgm.bipm.org/vim/en/1.6.html
[quantity]: http://jcgm.bipm.org/vim/en/1.1.html
[measurement]: http://jcgm.bipm.org/vim/en/1.9.html
[orbiter]: https://en.wikipedia.org/wiki/Mars_Climate_Orbiter

## Usage
`uom` requires `rustc` 1.15.0 or later. Add this to your `Cargo.toml`:

```toml
[dependencies]
uom = "0.13.0"
```

and this to your crate root:

```rust
extern crate uom;
```

The simple example below shows how to use quantities and units as well as how `uom` stops invalid
operations:

```rust
extern crate uom;

use uom::si::f32::*;
use uom::si::length::kilometer;
use uom::si::time::second;

fn main() {
	let length = Length::new::<kilometer>(5.0);
	let time = Time::new::<second>(15.0);
	let _velocity = length / time;
	//let error = length + time; // error[E0308]: mismatched types
}
```

See the [examples](examples) directory for more advanced usage:

 * [si.rs](examples/si.rs) -- Example showing how to use the pre-built SI system.
 * [base.rs](examples/base.rs) -- Example showing how to create a set of `Quantity` type aliases
   for a different set of base units.
 * [mks.rs](examples/mks.rs) -- Example showing how to create a custom system of quantities.

## Features
`uom` has four `Cargo` features: `f32`, `f64`, `si`, and `std`. The features are described below
and are enabled by default. Features can be cherry-picked by using the `--no-default-features` and
`--features "..."` flags when compiling `uom` or specifying features in Cargo.toml:

```toml
[dependencies]
uom = { version = "0.13.0", default-features = false, features = ["f32", "f64", "si", "std"] }
```

 * `f32`, `f64` -- Features to enable underlying storage types. At least one of these features must
   be enabled.
 * `si` -- Feature to include the pre-built [International System of Units][si] (SI).
 * `std` -- Feature to compile with standard library support. Disabling this feature compiles `uom`
   with `no_std`. Note that some functions such as `sqrt` require `std` to be enabled.

[si]: http://jcgm.bipm.org/vim/en/1.16.html

## Design
Rather than working with [measurement units](http://jcgm.bipm.org/vim/en/1.9.html) (meter,
kilometer, foot, mile, ...) `uom` works with [quantities](http://jcgm.bipm.org/vim/en/1.1.html)
(length, mass, time, ...). This simplifies usage because units are only involved at interface
boundaries: the rest of your code only needs to be concerned about the quantities involved. This
also makes operations on quantities (+, -, \*, /, ...) have zero runtime cost<sup>1</sup> over
using the raw storage type (e.g. `f32`).

`uom` normalizes values to the [base unit](http://jcgm.bipm.org/vim/en/1.10.html) for the quantity.
Alternative base units can be used by executing the macro defined for the system of quantities
(`ISQ!` for the SI). `uom` supports both `f32` and `f64` as the underlying storage type.

 1. Once codegen bug [#38269](https://github.com/rust-lang/rust/issues/38269) is resolved.

## License
Licensed under either of

 * Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or
   http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

### Contribution
Contributions are welcome from everyone. Submit a pull request, an issue, or just add comments to an
existing item. The [International Bureau of Weights and Measures][BIPM] is an international
standards organization that publishes the [SI Brochure][brochure]. This document defines the [SI]
and can be used as a comprehensive reference for changes to `uom`. Conversion factors for non-SI
units can be found in NIST [Special Publication 811][nist811].

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in
the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any
additional terms or conditions.

[BIPM]: http://www.bipm.org/en/about-us/
[brochure]: http://www.bipm.org/en/publications/si-brochure/
[si]: http://jcgm.bipm.org/vim/en/1.16.html
[nist811]: https://www.nist.gov/pml/nist-guide-si-appendix-b9-factors-units-listed-kind-quantity-or-field-science
