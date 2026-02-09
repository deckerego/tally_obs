# TallyOBS

[Official OBS Plugin Listing](https://obsproject.com/forum/resources/tallypi-push-scene-changes-to-wifi-enabled-tally-lights.1082/)

OBS scripts and web dashboards to control either
[TallyCircuitPy](https://github.com/deckerego/tally_circuitpy)
or [TallyPi](https://github.com/deckerego/tally_pi)
remotely via HTTP.

Rather than having the lights hit a web gateway hosted by OBS, TallyOBS reaches
out directly to the tally lights and controls them over HTTP. This does not use
the websocket plugin and there is no need to open up any firewall holes to your
OBS production box - instead the script pushes out commands directly to your lights.

<a href="http://www.youtube.com/watch?feature=player_embedded&v=eRcYikv5V9U" target="_blank">
 <img src="http://img.youtube.com/vi/eRcYikv5V9U/mqdefault.jpg" alt="Overview of TallyOBS / Pi / CircuitPy" width="320" height="240" border="10" />
</a>

## OBS Script

A Python [OBS script](./scripts/obs_tally_light.py) is provided that maps
preview/program/idle status to AV input sources. You can chose the color
and brightness for the status of your input sources, and map each input source
to the IP addresses or hostnames of your tally light web interface. If you want
to interacet with multiple lights, a comma-separated list of addresses
can be provided.

![OBS Plugin Settings](./docs/images/obs_settings.png)

You must have installed the correct version of Python for OBS to properly load
Python plugins. Details for setting up OBS, installing the interface,
and configuring settings are available at [OBS.md](./docs/OBS.md).

If you are having trouble identifying your lights by hostname, you can use either
the [HTML dashboard page](./scripts/dashboard.html) or the
[scripts/find_lights.sh](./scripts/find_lights.sh) command-line script to search
your network for available lights. Either option will provide you with an IPv4
address for each light found which can be used to setup the OBS plugin.


## Web Dashboard

A web dashboard is provided in [scripts/dashboard.html](./scripts/dashboard.html)
as a single HTML page that you can load directly in a browser - no server needed.
Enter in the IPv4 network you would like to search, and the page will quickly
crawl the network looking for API endpoints listening on port 7413. If it finds
any tally lights, it will display the IPv4 address, current color, and brightness
which can be changed directly in the dashboard.

![Tally Light Dashboard](./docs/images/dashboard.png)


## Supported Tally Light Hardware

TallyOBS can use either [TallyCircuitPy](https://github.com/deckerego/tally_circuitpy)
or [TallyPi](https://github.com/deckerego/tally_pi) hardware. This includes
(but isn't limited to):

- Raspberry Pi Zero W (all versions)
- Raspberry Pi (all versions)
- ODT's [PixelWing ESP32-S2 RGB Matrix](https://www.tindie.com/products/oakdevtech/pixelwing-esp32-s2-rgb-matrix/)
- Other hardware supported by CircuitPython (testing underway with Adafruit Featherwing hardware)

In general the ESP32-S2 hardware used by [TallyCircuitPy](https://github.com/deckerego/tally_circuitpy),
however if you have a drawer full of Raspberry Pi's [TallyPi](https://github.com/deckerego/tally_pi)
works extrodinarily well.


## The Tally Light API

Both [TallyCircuitPy](https://github.com/deckerego/tally_circuitpy)
and [TallyPi](https://github.com/deckerego/tally_pi)
share a common standard for controlling their LED arrays. An HTTP interface is
provided with each that allows for color control and brightness
to be specified remotely. As an example:

    http://192.168.1.1:7413/set?color=AA22FF&brightness=0.3

Would set the LED array to be purple at 30% brightness.
The status of the LEDs are available as:

    http://192.168.1.1:7413/status
