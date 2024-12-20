# hoymiles-wifi

This Python library facilitates communication with Hoymiles DTUs and the HMS-XXXXW-2T HMS microinverters, utilizing protobuf messages.

For the Home Assistant integration have a look here:
https://github.com/suaveolent/ha-hoymiles-wifi

**Disclaimer: This library is not affiliated with Hoymiles. It is an independent project developed to provide tools for interacting with Hoymiles HMS-XXXXW series micro-inverters featuring integrated WiFi DTU. Any trademarks or product names mentioned are the property of their respective owners.**

## Supported Devices

The library was successfully tested with:

- Hoymiles HMS-400W-1T
- Hoymiles HMS-800W-2T
- Hoymiles DTU-WLite
- Hoymiles DTU-Pro

## Installation

```
$ pip install hoymiles-wifi
```

## Usage

You can integrate the library into your own project, or simply use it in the command line.

### Command line:

```
hoymiles-wifi [-h] --host HOST [--local_addr IP_OF_INTERFACE_TO_USE] [--as-json] <command>

commands:
    get-real-data-new,
    get-real-data,
    get-config,
    network-info,
    app-information-data,
    app-get-hist-power,
    set-power-limit,
    set-wifi,
    firmware-update,
    restart-dtu,
    turn-on-inverter,
    turn-off-inverter,
    get-information-data,
    get-version-info,
    heartbeat,
    identify-dtu,
    identify-inverters,
    identify-meters,
    get-alarm-list,
    enable-performance-data-mode,

The `--as-json` option is optional and allows formatting the output as JSON.
```

### Python code

```
from hoymiles_wifi.dtu import DTU
...
dtu = DTU(<ip_address>)
response = await dtu.<command>

if response:
    print(f"DTU Response: {response}")
else:
    print("Unable to get response!")
```

#### Available functions

- `async_get_real_data_new()`: Retrieve real-time data
- `async_get_real_data()`: Retrieve real-time data
- `async_get_config()`: Retrieve configuration information
- `async_network_info()`: Retrieve network information
- `async_app_information_data()`: Retrieve application information data
- `async_app_get_hist_power()`: Retrieve historical power data
- `async_set_power_limit(power_limit)`: Set the power limit of the inverter (0-100%)
- `async_set_wifi(wifi_ssid, wifi_password)`: Configure the wifi network
- `async_firmware_update()`: Update to latest firmware
- `async_restart_dtu()`: Restart the DTU
- `async_turn_on_inverter()`: Turn the inverter on
- `async_turn_off_inverter()`: Turn the inverter off
- `async_get_information_data()`: Retrieve information data
- `async_heartbeat()`: Request a heartbeat message from the DTU
- `async_get_alarm_list()`: Get alarm list from the DTU
- `async_enable_performance_data_mode()`: Enabel a higher update interval (every ~30s or less)

## Note

Please be aware of the following considerations:

- No DTU Implementation: This library retrieves information directly from the (internal) DTU of Hoymiles Wifi inverters.

## Caution

Use this library responsibly and be aware of potential risks. There are no guarantees provided, and any misuse or incorrect implementation may result in undesirable outcomes. Ensure that your inverter is not compromised during communication.

## Known Limitations

### Update Frequency:
> [!NOTE]
>  The library may experience limitations in fetching updates, around twice per minute. This shows as the Data you recive back shows every time the same as the previos request. If this happens to you than you have 3 options:
> 1. lower your update interval to ~35s between requestss.
> 2. use `async_enable_performance_data_mode()`
> 3. open the S-Miles Installer App, conect to DTU and open the Toolkit (sends the same Data as 2.)
>
>This behavior apers to reset after each (re-)start!

> [!IMPORTANT]
>Setting the update interval below approximately 32 seconds may disable Hoymiles cloud functionality. To ensure proper communication with Hoymiles servers, keep the update interval at or above this threshold.

> [!CAUTION]
> With a even higher update interval (1~2s) you can also also lose connectivity with the App.

### Compatibility:
> [!IMPORTANT]
> While developed for the HMS-800W-2T inverter, compatibility with other inverters from the series is untested at the time of writing. Exercise caution and conduct thorough testing if using with different inverter models.

## Attribution

A special thank you for the inspiration and codebase to:

- [DennisOSRM](https://github.com/DennisOSRM): [hms-mqtt-publisher](https://github.com/DennisOSRM/hms-mqtt-publisher)
- [henkwiedig](https://github.com/henkwiedig): [Hoymiles-DTU-Proto](https://github.com/henkwiedig/Hoymiles-DTU-Proto)
