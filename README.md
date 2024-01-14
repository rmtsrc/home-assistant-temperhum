# Home Assistant TEMPerHUM USB

This Home Assistant integration `temperhum` sensor platform allows you to get the current temperature and humidity from [TEMPer](https://www.google.com/search?q=TEMPer+USB) and [TEMPerHUM](https://www.google.com/search?q=TEMPerHUM+USB) USB devices.

## Installation

### HACS (recommended)

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg?style=for-the-badge)](https://github.com/hacs/integration)

1. Add this repos URL to the **Custom repositories** list from the hamburger menu and choose **Integration**.
2. Search for **TEMPerHUM** via the HACS UI and click the **Download** button (bottom left)

*[HACS](https://hacs.xyz/) is a third party community store and is not included in Home Assistant out of the box and allows you to easily install, update and remove custom components. If you've not already installed HACS in Home Assistant view [their installation process](https://hacs.xyz/docs/setup/prerequisites).*

### Manual

Download the latest [temperhum.zip](https://github.com/rmtsrc/home-assistant-temperhum/releases/latest/download/temperhum.zip) from the [releases page](https://github.com/rmtsrc/home-assistant-temperhum/releases) and extract the contents into your Home Assistants `config/custom_components/temperhum` folder.

## Configuration

To use your TEMPer and/or TEMPerHUM USB sensor in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: temperhum
```

Optional configuration:

```yaml
# Example of a configuration.yaml with optional entries added
sensor:
  - platform: temperhum
    # description: The name to use when displaying this switch.
    # required: false
    # type: string
    # default: ID of the device
    name:
    # description: The offset to fix reported vales.
    # required: false
    # type: integer
    # default: 0
    offset:
    # description: The scale for the sensor.
    # required: false
    # type: integer
    # default: 1
    scale:
```

After restarting Home Assistant with the new configuration the sensors can be found in the auto generated dashboard or if you've customised it, the sensors can be added by editing your dashboard, adding a new card by entity and searching for **TEMPer**.

Since some of these sensors consistently show higher temperatures the scale and offset values can be used to fine-tune your sensor.
The calculation follows the formula `scale * sensor value + offset`.

The TEMPer sensors can only be accessed as root by default. To fix the USB permissions on your system create the file `/etc/udev/rules.d/99-tempsensor.rules` and add the following line to it:

```text
SUBSYSTEMS=="usb", ACTION=="add", ATTRS{idVendor}=="0c45", ATTRS{idProduct}=="7401", MODE="666"
```

After that re-plug the device and restart Home Assistant.

Note: this custom component is based on [home-assistant/core#85082](https://github.com/home-assistant/core/pull/85082).
