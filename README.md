# `gyuvl53l0x`

> no_std driver for [VL53L0X](https://www.st.com/resource/en/datasheet/vl53l0x.pdf) (Time-of-Flight I2C laser-ranging module)

[![Build Status](https://travis-ci.org/lucazulian/gyuvl53l0x.svg?branch=master)](https://travis-ci.org/lucazulian/gyuvl53l0x)
[![crates.io](http://meritbadge.herokuapp.com/gyuvl53l0x?style=flat-square)](https://crates.io/crates/gyuvl53l0x)

## Basic usage

Include this [library](https://crates.io/crates/gyuvl53l0x) as a dependency in your `Cargo.toml`:

```rust
[dependencies.gyuvl53l0x]
version = "<version>"
```

Use [embedded-hal](https://github.com/rust-embedded/embedded-hal) implementation to get I2C handle and then create vl53l0x handle:

```rust
extern crate gyuvl53l0x;

match gyuvl53l0x::VL53L0X::default(&mut i2c) {
    Ok(mut u) => {
        u.set_device_address(&mut i2c, 0x39).unwrap();
        loop {
            match u.read_range_single_millimeters_blocking(&mut i2c) {
                Ok(a) => {
                    println!("{:#?}", a).unwrap();
                }
                _ => {
                    println!("Not ready").unwrap();
                }
            }
        }
    }
    Err(gyuvl53l0x::VL53L0X::Error::BusError(error)) => {
        println!("{:#?}", error).unwrap();
        panic!();
    }
    _ => {
        panic!();
    }
};
```

## Documentation

API Docs available on [docs.rs](https://docs.rs/gyuvl53l0x/0.1.3/gyuvl53l0x/)

## License

[MIT license](http://opensource.org/licenses/MIT)
