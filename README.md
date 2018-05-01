# Snow

[![Crates.io](https://img.shields.io/crates/v/snow.svg)](https://crates.io/crates/snow)
[![Docs.rs](https://docs.rs/snow/badge.svg)](https://docs.rs/snow)
[![Build Status](https://travis-ci.org/mcginty/snow.svg?branch=master)](https://travis-ci.org/mcginty/snow)

![totally official snow logo](https://i.imgur.com/gFgvo49.jpg?1)

An implementation of Trevor Perrin's [Noise Protocol](https://noiseprotocol.org/) that is designed to be
Hard To Fuck Up™.

🔥 **Warning** 🔥 This library has not received any formal audit, and its API is subject to change whenever it's
prudent to or if the winds blow at the right heading.

## What's it look like?
See `examples/simple.rs` for a more complete TCP client/server example.

```rust
let mut noise = NoiseBuilder::new("Noise_NN_ChaChaPoly_BLAKE2s".parse()?)
                    .build_initiator()?;
 
let mut buf = [0u8; 65535];
 
// write first handshake message
noise.write_message(&[], &mut buf)?;
 
// receive response message
let incoming = receive_message_from_the_mysterious_ether();
noise.read_message(&incoming, &mut buf)?;
 
// complete handshake, and transition the state machine into transport mode
let mut noise = noise.into_transport_mode()?;
```

See the full documentation at [https://docs.rs/snow](https://docs.rs/snow).


## Implemented

Snow is currently feature-frozen on its current rev32 implementation, since revision 33 will
be releasing shortly.

- [x] Rekey()
- [x] `pskN` modifier
- [ ] specifying PSKs after building `Session`
- [ ] `fallback` modifier

## Crypto
Cryptographic providers are swappable through `NoiseBuilder::with_provider()`, but by default it chooses select, artisanal
pure-Rust implementations (see `Cargo.toml` for a quick overview).

### Acceleration

If you enable the `ring-accelerated` feature, Snow will default to choosing `ring`'s *much* faster crypto implementations when supported.

If you enable the `ring-resolver` feature, Snow will include a ring_wrapper module as well as a `RingAcceleratedResolver` available to be used with `NoiseBuilder::with_resolver()`.

