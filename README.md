thrift_codec
============

[![thrift_codec](https://img.shields.io/crates/v/thrift_codec.svg)](https://crates.io/crates/thrift_codec)
[![Documentation](https://docs.rs/thrift_codec/badge.svg)](https://docs.rs/thrift_codec)
[![Actions Status](https://github.com/sile/thrift_codec/workflows/CI/badge.svg)](https://github.com/sile/thrift_codec/actions)
[![Coverage Status](https://coveralls.io/repos/github/sile/thrift_codec/badge.svg)](https://coveralls.io/github/sile/thrift_codec)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

This crate provides functionalities for encoding/deconding [Thrift][thrift] protocol.

[Documentation](https://docs.rs/thrift_codec)

References
----------

- [Thrift Protocol Structure][protocol-structure]
- [Thrift Binary protocol encoding][binary-encoding]
- [Thrift Compact protocol encoding][compact-encoding]

[thrift]: https://thrift.apache.org/
[protocol-structure]: https://github.com/apache/thrift/blob/master/doc/specs/thrift-protocol-spec.md
[binary-encoding]: https://github.com/apache/thrift/blob/master/doc/specs/thrift-binary-protocol.md
[compact-encoding]: https://github.com/apache/thrift/blob/master/doc/specs/thrift-compact-protocol.md

Examples
--------

Encodes a message:

```rust
use thrift_codec::CompactEncode;
use thrift_codec::data::Struct;
use thrift_codec::message::Message;

let message = Message::oneway("foo_method", 1, Struct::from(("arg1", 2)));
let mut buf = Vec::new();
message.compact_encode(&mut buf).unwrap();
assert_eq!(
    buf,
    [130, 129, 1, 10, 102, 111, 111, 95, 109, 101, 116,
    104, 111, 100, 24, 4, 97, 114, 103, 49, 21, 4, 0]
);
```

Decodes the above binary:

```rust
use thrift_codec::CompactDecode;
use thrift_codec::data::Struct;
use thrift_codec::message::Message;

let bytes = [
    130, 129, 1, 10, 102, 111, 111, 95, 109, 101, 116,
    104, 111, 100, 24, 4, 97, 114, 103, 49, 21, 4, 0
];

let message = Message::compact_decode(&mut &bytes[..]).unwrap();
let expected = Message::oneway("foo_method", 1, Struct::from(("arg1", 2)));
assert_eq!(message, expected);
```
