# Integrating Rust into Existing Projects

I'd like to write up some documentation and best practices for integrating
[Rust](https://www.rust-lang.org/) into existing software projects. I expect
most of the focus will be on integrating into C and C++ projects, but there's
certainly room for information about integrating Rust into projects written in
other languages.

As a first cut I'm just going to link the existing work I know about in this
space.

## Firefox

I've been heavily involved in the planning work of integrating Rust code into Firefox. We are shipping some Rust code (an mp4 parser) currently, but it's not enabled by default if you build Firefox from source. There are large in-progress
projects to use parts of [Servo](https://servo.org/) in Firefox which will
bring in a lot more Rust code. These projects are all working with the
infrastructure we built, so our foundation seems sound.

### Build System

Firefox uses autoconf and GNU make (not automake), but with a lot of custom
scaffolding. We elected to build our Rust code using `cargo` to make things
work as expected for Rust developers. This has worked very well once we
ironed out a few wrinkles. There's [some documentation](http://gecko.readthedocs.io/en/latest/build/buildsystem/rust.html)
on how to add new Rust code to Firefox available. Under the hood the build
system mostly just [invokes cargo](https://dxr.mozilla.org/mozilla-central/rev/5e76768327660437bf3486554ad318e4b70276e1/config/rules.mk#935)
to build a static library containing all the Rust code, then links that
into the final binary.

### Vendoring

We elected to vendor all Rust crate dependencies from crates.io into our
repository, since the historical position has been that the Firefox build
should not need to touch the internet. We used [`cargo vendor`](https://github.com/alexcrichton/cargo-vendor/) to do so. This requires the cargo that
shipped with Rust 1.12 with support for source replacement.

## VLC

Geoffroy Couprie [gave a talk at RustConf 2016](http://dev.unhandledexpression.com/slides/rustconf-2016/vlc/)
about integrating Rust into the VLC media player. He wrote [a VLC plugin](https://github.com/Geal/rust-vlc-demux) in Rust.

### Build System

VLC uses automake. Geoffroy integrated the Rust code as a static library,
built by cargo, using standard automake boilerplate. (TODO: expand on this)

### Integration with existing C APIs

A large part of the presentation was on the challenges of integrating with the
existing C APIs. TODO: write up some of that.

## GStreamer

Sebastian Dröge [gave a talk at GStreamer Conference 2016](https://gstconf.ubicast.tv/videos/corroded-pipelines-or-how-to-write-gstreamer-element-in-rust/)
on writing a GStreamer element in Rust. He also wrote a pair of blog posts on it: [one](https://coaxion.net/blog/2016/05/writing-gstreamer-plugins-and-elements-in-rust/), [two](https://coaxion.net/blog/2016/09/writing-gstreamer-elements-in-rust-part-2-dont-panic-we-have-better-assertions-now-and-other-updates/).
The [GStreamer plugin he wrote](https://github.com/sdroege/rsplugin) is available.

TODO: write up some of the interesting bits.

