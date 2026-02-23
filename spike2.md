# Spike 2

## The `hub` and `spike` module

Documentation for the `hub` module, offering more advanced functions compared to the `spike` (which is just a wrapper around `hub`) modules can be found on these two sites, mostly overlapping, but some things are exclusively on one of them:

- <https://hubmodule.readthedocs.io/en/latest/>
- <https://lego.github.io/MINDSTORMS-Robot-Inventor-hub-API/>

Some more analysis of this can be found here, although much is left unclear or missing: <https://www.antonsmindstorms.com/2021/01/14/advanced-undocumented-python-in-spike-prime-and-mindstorms-hubs/>  
I also found this, but I don't think it adds anything substantial: <https://tuftsceeo.github.io/SPIKEPythonDocs/SPIKE2.html>

All motors and sensors can be read from the device & motor API (`hub.port.X.device` & `hub.port.X.motor`) directly instead of using the `spike` functions specific to a sensor or motor, as seen here: <https://hubmodule.readthedocs.io/en/latest/sensors/>  
This sometimes allows for more precise/faster reading of the sensors. We have seen a massive improvement of our Robot Game performance/reproducability after switching to these with our own wrappers instead of using the `spike` module.

The `spike` module, and generally all modules, are included in the filesystem as compiled mpy files.

## The firmware

Details on the firmware and the firmware itself can be found here: [gpdaniels/spike-prime](https://github.com/gpdaniels/spike-prime) (but excluding SPIKE3 Software)  
The firmware is located directly in the `/` (root) folder, and the runtime is called from `/main.py`. If you overwrite this file, you can run your own code instread of LEGO's official runtime.

Generally, the [micropython documentation](https://docs.micropython.org/en/latest/library/bluetooth.html) can be used, although some modules may be missing. Also, the SPIKE2 firmware uses older versions of micropython, so the documentation may be too up-to-date sometimes.

## Communication

The `hub` module offers `hub.USB_VCP` and `hub.BT_VCP`, which allow you to communicate over USB and Bluetooth relatively easy, respectively. I have not checked if Bluetooth Low Energy is possible.

The communication to the SPIKE App happens via a JSON RPC API over wither USB or BT. I have reimplemented parts of it in [spike-prime-connect](https://github.com/GSG-Robots/spike-prime-connect), based on <https://github.com/sanjayseshan/spikeprime-tools> and <https://github.com/PeterStaev/lego-spikeprime-mindstorms-vscode/tree/legacy>.

## Buttons

This can be read in the documentation of the `hub` module, but this is the reason all of this started, so I wanted to include this.

We wanted to use the center button.  
The trick is this:

```python
import hub

# That the center button closes the running program is implemented via a button callback. A button can only ever have one button callback.

# We read the normal callback for later use
center_callback = hub.button.center.callback()

# And we remove the callback
hub.button.center.callback(None)


# Now, here you place the actual program.
# It is a good idea to place this in a try-finally block, so the callback can always be reverted to the initial value. If you don't revert the callback, the center button will not work unless you reboot the hub.
... # Use hub.button.center.is_pressed() and hub.button.center.was_pressed()
    # here to check the center button

# At the end, we want to reenable the normal functionality of the center button:
hub.button.center.callback(center_callback)
```