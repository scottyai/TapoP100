# PyP100

This fork of the PyP100 is cutdown exclude anything to do with Tapo lights and is dedicated to using the P series plugs and more specifically to publish onto a MQTT broker for collection to a time series database. 

PyP100 is a Python library for controlling TP-Link Tapo smart devices including P100, P105, P110 plugs.

**Compatibility:** Supports firmware version 1.2 and above with automatic fallback to older authentication methods.

> [!IMPORTANT]
> For firmware 1.4.0 and above, you must enable "Third-Party Compatibility" in the Tapo app.
>
> Go to: **Me > Third-Party Services > Third-Party Compatibility** and toggle it on.

## Installation

PyP100 can be installed using the package manager [pip](https://pip.pypa.io/en/stable/).

```bash
pip install git+https://github.com/scottyai/TapoP100.git@main
```

> **Note:** If you previously installed PyP100 from another source, use `--force-reinstall` to ensure you get this version:
> ```bash
> pip install --force-reinstall git+https://github.com/scottyai/TapoP100.git@main
> ```

## Usage

#### Plugs - P100, P105 etc.

```python
from PyP100 import PyP100

p100 = PyP100.P100("192.168.X.X", "email@gmail.com", "Password123")  # Creates a P100 plug object

p100.turnOn()  # Turns the connected plug on
p100.turnOff()  # Turns the connected plug off
p100.toggleState()  # Toggles the state of the connected plug

p100.getDeviceInfo()  # Returns dict with all the device info of the connected plug
p100.getDeviceName()  # Returns the name of the connected plug set in the app
p100.getCountDownRules()  # Returns the countdown rule. Useful to check if you need to update them

p100.handshake()  # DEPRECATED
p100.login()  # DEPRECATED
```

#### Old Authentication Method

The old authentication method is used as a fallback if the new authentication method fails. It can be forced by setting
the `preferred_protocol` parameter to "old" when creating the plug object.

```python
from PyP100 import PyP100

p100 = PyP100.P100("192.168.X.X", "email@gmail.com", "Password123",
                   preferred_protocol="old")  # Creates a P100 plug object using the old authentication method only
```

#### Energy Monitoring - P110

```python
from PyP100 import PyP110

p110 = PyP110.P110("192.168.X.X", "email@gmail.com", "Password123")

# The P110 has all the same basic functions as the plugs and additionally allow for energy monitoring.
p110.getEnergyUsage()  # Returns dict with all of the energy usage of the connected plug
p110.getEnergyData(1706825847, 1708643847, MeasureInterval.DAYS) # Returns power consumption per day since 1st Feb 24
```

If you call `getEnergyData` function, power consumption could be collected per `HOURS`, `DAYS` or `MONTHS` interval. The start timestamp is ([most probably](https://github.com/fishbigger/TapoP100/pull/87#issuecomment-1565334341)) rounded to the midnight, the first day of month or the first of January based on interval.

## Contributing

Contributions are always welcome!

Please submit a pull request or open an issue for any changes.

Thanks to [OctoPrint-PSUControl-Tapo](https://github.com/dswd/OctoPrint-PSUControl-Tapo) project.

## License

[MIT](https://choosealicense.com/licenses/mit/)
