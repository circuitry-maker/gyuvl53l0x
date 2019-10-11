# `vl53l0x`

> no_std driver for [VL53L0X](https://www.st.com/resource/en/datasheet/vl53l0x.pdf) (Time-of-Flight I2C laser-ranging module)

## Basic usage

Include this library as a dependency in your `Cargo.toml`:

```
[dependencies.vl53l0x]
version = "<version>"
```

Use [embedded-hal](https://github.com/rust-embedded/embedded-hal) implementation to get I2C handle and then create vl53l0x handle:

```rust
extern crate vl53l0x;

match vl53l0x::VL53L0x::new(i2c) {
    Ok(mut u) => {
        loop {
            match u.read_range_single_millimeters_blocking() {
                Ok(a) => {
                    println!("{:#?}", a);
                }
                _ => {
                    println!!("Not ready").unwrap();
                }
            }
        }
    }
    Err(vl53l0x::Error::BusError(error)) => {
        println!!("{:#?}", error);
    }
};
```

## Documentation

API Docs available on [docs.rs]()

## License

[MIT license](http://opensource.org/licenses/MIT)
