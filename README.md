# `gyuvl53l0x`

> no_std driver for [VL53L0X](https://www.st.com/resource/en/datasheet/vl53l0x.pdf) (Time-of-Flight I2C laser-ranging module)

[![Build Status](https://github.com/lucazulian/gyuvl53l0x/workflows/gyuvl53l0x-ci/badge.svg)](https://github.com/lucazulian/gyuvl53l0x/actions?query=workflow%3Agyuvl53l0x-ci)
[![crates.io](http://meritbadge.herokuapp.com/gyuvl53l0x?style=flat-square)](https://crates.io/crates/gyuvl53l0x)
[![Docs](https://docs.rs/gyuvl53l0x/badge.svg)](https://docs.rs/gyuvl53l0x)

## Basic usage

Include this [library](https://crates.io/crates/gyuvl53l0x) as a dependency in your `Cargo.toml`:

```rust
[dependencies.gyuvl53l0x]
version = "<version>"
```

Use [embedded-hal](https://github.com/rust-embedded/embedded-hal) implementation to get I2C handle and then create vl53l0x handle.

Single read:

```rust
extern crate gyuvl53l0x;

match gyuvl53l0x::VL53L0X::default(i2c) {
    Ok(mut u) => {
        // set a new device address
        u.set_device_address(0x39).unwrap();
        // set the measurement timing budget in microseconds
        u.set_measurement_timing_budget(20_000).unwrap();
        loop {
            match u.read_range_single_millimeters_blocking() {
                Ok(val) => {
                    println!("{:#?}", val).unwrap();
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

Continuos read:

```rust
extern crate gyuvl53l0x;

match gyuvl53l0x::VL53L0X::default(i2c) {
    Ok(mut u) => {
        u.start_continuous(20_000).unwrap();
        loop {
            match u.read_range_continuous_millimeters_blocking() {
                Ok(val) => {
                    println!("{:#?}", val).unwrap();
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

## License

[MIT license](http://opensource.org/licenses/MIT)
