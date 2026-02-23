# Spike 3

The spike 3 firmware has the advantage of being 10x more precise in most places, and running even faster than the `hub` API of SPIKE 2.  
Unfortunately, for SPIKE3, there is much less documentation available than for SPIKE2.

## The `hub` module

For SPIKE3, only one module `hub` exists, and unlike with SPIKE2, all modules by LEGO (`_system.*` and `hub`) are embedded into the firmware rather than included in the filesystem, which makes it harder to look at them and impossible to remove.

## Communication

The SPIKE3 firmware does not offer Bluetooth support, only Bluetooth Low Energy via the [micropython bluetooth module](https://docs.micropython.org/en/latest/library/bluetooth.html). Serial communication over USB can be done via stdin and stdout (ie. `input()`, `print()`, `sys.stdin.read()` and `sys.stdout.write()`).

The official protocol to communicate with the HUB over BLE is documented here: <https://lego.github.io/lego-ble-wireless-protocol-docs/index.html>

## The firmware

Here, the [micropython docs](https://docs.micropython.org/en/latest/library/index.html) generally apply and are up-to-date, but many modules are missing.

### sdf

- https://tuftsceeo.github.io/SPIKEPythonDocs/SPIKE3.html