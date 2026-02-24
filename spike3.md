# Spike 3

The spike 3 firmware has the advantage of being 10x more precise in most places, and running even faster than the `hub` API of SPIKE 2.  
Unfortunately, for SPIKE3, there is much less documentation available than for SPIKE2.

## The `hub` module

For SPIKE3, only one module `hub` exists, and unlike with SPIKE2, all modules by LEGO (`_system.*` and `hub`) are embedded into the firmware rather than included in the filesystem, which makes it harder to look at them and impossible to remove.

What I believe to be a copy of the official documentation found in the app can be found here: <https://tuftsceeo.github.io/SPIKEPythonDocs/SPIKE3.html>. Note that this official documentation contains multiple errors. Best is to always ignore the summary & example of each module and go directly to the functions listed below, this way you should be fine.

## Communication

The SPIKE3 firmware does not offer Bluetooth support, only Bluetooth Low Energy via the [micropython bluetooth module](https://docs.micropython.org/en/latest/library/bluetooth.html). Serial communication over USB can be done via stdin and stdout (ie. `input()`, `print()`, `sys.stdin.read()` and `sys.stdout.write()`).

The official protocol to communicate with the HUB over BLE is documented here: <https://lego.github.io/lego-ble-wireless-protocol-docs/index.html>

## The firmware

Here, the [micropython docs](https://docs.micropython.org/en/latest/library/index.html) generally apply and are up-to-date, but many modules are missing.

All files are placed in `/flash`, so the `main.py` is at `/flash/main.py`.

The hub has a `hub.config` object which acts as a dictionary, but the keys cannot be listed using `dir()`. However, they are listed in the general help message, provided by the `help()` command in the repl. It states these config options:

- `hub.config["hub_name"]`  
  The name of the hub. This is the only persistant option and it is stored at `/flash/config/hub_name`.
- `hub.config["usb_force_power_on"]`  
  This is a boolean and determines whether the hub should automatically power on when it is connected to power over usb.
- `hub.config["hub_os_enable"]`  
  This is a boolean and determines whether the hub should boot into the HubOS, the official LEGO runtime.
- ... TODO

All options except the hub name are only stored until the next reboot and need to be placed in `boot.py` to persist.

If HubOS is disabled, you can place your own runtime in `/flash/main.py`
