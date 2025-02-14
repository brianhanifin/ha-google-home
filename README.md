[![GitHub Workflow Status][workflow-shield]][workflow]
[![Contributors][contributors-shield]][contributors]
[![hacs][hacsbadge]][hacs]
[![GitHub Release][releases-shield]][releases]
[![GitHub Activity][commits-shield]][commits]
[![License][license-shield]][license]

<p align="center">
  <a href="https://github.com/leikoilja/ha-google-home">
    <img src="https://brands.home-assistant.io/google_home/icon.png" alt="Logo" height="200">
  </a>
</p>

<h3 align="center">Home Assistant Google Home community integration</h3>

<p align="center">
  This custom integration aims to provide plug-and-play Google Home
  experience for Home Assistant enthusiasts.
</p>

**[!] Beta version alert.**
Please note this integration is in the early stage of it's development.
See [Contribution](#contribution) section for more information.

<details open="open">
  <summary>Table of Contents</summary>

1. [About The Project](#about)
2. [Features](#features)
3. [Getting Started](#getting-started)
   - [Prerequisites](#prerequisites)
   - [HACS Installation](#hacs-installation)
   - [Manual Installation](#manual-installation)
     - [Git clone method](#git-clone-method)
     - [Copy method](#copy-method)
   - [Integration Setup](#integration-setup)
   - [Running in Home Assistant Docker container](#running-in-home-assistant-docker-container)
4. [Lovelace Cards](#lovelace-cards)
5. [Contribution](#contribution)
6. [Credits](#credits)

</details>

## About

This is a custom component that is emerging from the
[community discussion][community-discussion] of a need to be able to retrieve
local google assistant device (like Google Home/Nest etc) authentication
tokens and use those tokens making API calls to Google Home devices.

## Features

This component will set up the following platforms:

| Platform | Sample sensor                   | Description                                               |
| -------- | ------------------------------- | --------------------------------------------------------- |
| `sensor` | `sensor.living_room_timers`     | Sensor with a list of timers from the device              |
| `sensor` | `sensor.living_room_next_timer` | Sensor with next timer from the device                    |
| `sensor` | `sensor.living_room_alarms`     | Sensor with a list of alarms from the device              |
| `sensor` | `sensor.living_room_next_alarm` | Sensor with next alarm from the device                    |
| `sensor` | `sensor.living_room_token`      | Sensor with the local authentication token for the device |

### Timers

You can have multiple timers on your Google Home device. The Home Assistant
timers sensor will represent all of them in the state attributes as a list "timers".
Each of timers has the following keys:

| Key              | Value type                   | Description                                                               |
| ---------------- | ---------------------------- | ------------------------------------------------------------------------- |
| `id`             | Google Home corresponding ID | Used to delete/modify timer                                               |
| `label`          | Name                         | Name of the timer, this can be set when making the timer                  |
| `fire_time`      | Seconds                      | Raw value coming from Google Home device until the timer goes off         |
| `local_time`     | Time                         | Time when the timer goes off, in respect to the Home Assistant's timezone |
| `local_time_iso` | Time in ISO 8601 standard    | Useful for automations                                                    |
| `duration`       | Seconds                      | Timer duration in seconds                                                 |

### Next timer

Will always show the next timer as timestring (ie: `2021-03-07T15:26:17+01:00`) if there is one set, else it will show off.

Has the same attributes as `sensor.living_room_timers`, but only for the first timer.

### Alarms

You can have multiple alarms on your Google Home device. The Home Assistant
alarms sensor will represent all of them in the state attributes as a list
"alarms".
Each of alarms has the following keys:

| Key              | Value type                   | Description                                                                                                                                                                                             |
| ---------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`             | Google Home corresponding ID | Used to delete/modify alarm                                                                                                                                                                             |
| `label`          | Name                         | Name of the alarm, this can be set when making the alarm                                                                                                                                                |
| `fire_time`      | Seconds                      | Raw value coming from Google Home device until the alarm goes off                                                                                                                                       |
| `local_time`     | Time                         | Time when the alarm goes off, in respect to the Home Assistant's timezone                                                                                                                               |
| `local_time_iso` | Time in ISO 8601 standard    | Useful for automations                                                                                                                                                                                  |
| `recurrence`     | List of integers             | Days of the week when the alarm will go off. Please note, respecting Google set standard, the week starts from Sunday, therefore is denoted by 0. Correspondingly, Monday is 1, Saturday is 6 and so on |

### Next alarm

Will always show the next alarm as timestring (ie: `2021-03-07T15:26:17+01:00`) if there is one set, else it will show off.

This sensor is formatted to have compatibly with the sensor that mobile app has `sensor.phone_next_alarm`.

Has the same attributes as `sensor.living_room_alarms`, but only for the first alarm.

## Getting Started

### Prerequisites

Use Home Assistant build 2021.3 or above.

### HACS Installation

You can find it in the default HACS repo. Just search `Google Home`.

### Manual Installation

1. Open the directory with your Home Assistant configuration (where you find `configuration.yaml`,
   usually `~/.homeassistant/`).
2. If you do not have a `custom_components` directory there, you need to create it.

#### Git clone method

This is a preferred method of manual installation, because it allows you to keep the `git` functionality,
allowing you to manually install updates just by running `git pull origin master` from the created directory.

Now you can clone the repository somewhere else and symlink it to Home Assistant like so:

1. Clone the repo.

```shell
git clone https://github.com/leikoilja/ha-google-home.git
```

2. Create the symlink to `google_home` in the configuration directory.
   If you have non standard directory for configuration, use it instead.

```shell
ln -s ha-google-home/custom_components/google_home ~/.homeassistant/custom_components/google_home
```

#### Copy method

1. Download [ZIP](https://github.com/leikoilja/ha-google-home/archive/master.zip) with the code.
2. Unpack it.
3. Copy the `custom_components/google_home/` from the unpacked archive to `custom_components`
   in your Home Assistant configuration directory.

### Integration Setup

- Browse to your Home Assistant instance.
- In the sidebar click on [Configuration](https://my.home-assistant.io/redirect/config).
- From the configuration menu select: [Integrations](https://my.home-assistant.io/redirect/integrations).
- In the bottom right, click on the [Add Integration](https://my.home-assistant.io/redirect/config_flow_start?domain=ha-google-home) button.
- From the list, search and select “Google Home”.
- Follow the instruction on screen to complete the set up.
- After completing, the Google Home integration will be immediately available for use.

### Running in Home Assistant Docker container

Make sure that you have your Home Assistant Container network set to 'host', as perscribed in the official docker installation for Home Assistant.

## Lovelace Cards

**Open a PR to add your card here!**

- [Google Home timers card](https://github.com/DurgNomis-drol/google_home_timers_card) by [@DurgNomis-drol](https://github.com/DurgNomis-drol)

## Contribution

This integration is still in the early stage of it's development. Please use it
at your own risk. If you encounter issues or have any suggestions consider
opening issues and contributing through PR. If you are ready to contribute to this please read the [Contribution guidelines](CONTRIBUTING.md)

## Credits

This project was generated from [@oncleben31](https://github.com/oncleben31)'s [Home Assistant Custom Component Cookiecutter](https://github.com/oncleben31/cookiecutter-homeassistant-custom-component) template.

Code template was mainly taken from [@Ludeeus](https://github.com/ludeeus)'s [integration_blueprint][integration_blueprint] template.

Under the hood the integration uses [glocaltokens](https://github.com/leikoilja/glocaltokens) python package.

---

[buymecoffee]: https://www.buymeacoffee.com/leikoilja
[buymecoffeebadge]: https://img.shields.io/badge/buy%20me%20a%20coffee-donate-yellow.svg?style=for-the-badge
[commits-shield]: https://img.shields.io/github/commit-activity/y/leikoilja/ha-google-home.svg?style=for-the-badge
[commits]: https://github.com/leikoilja/ha-google-home/commits/master
[community-discussion]: https://community.home-assistant.io/t/solution-to-track-your-google-home-alarms-and-timers-and-trigger-different-home-assistant-events/61534/74
[contributors-shield]: https://img.shields.io/github/contributors/leikoilja/ha-google-home?style=for-the-badge
[contributors]: https://github.com/leikoilja/ha-google-home/graphs/contributors
[hacs]: https://hacs.xyz
[hacsbadge]: https://img.shields.io/badge/HACS-Custom-orange.svg?style=for-the-badge
[integration_blueprint]: https://github.com/custom-components/integration_blueprint
[license-shield]: https://img.shields.io/github/license/leikoilja/ha-google-home.svg?style=for-the-badge
[license]: https://github.com/leikoilja/ha-google-home/blob/master/LICENSE
[releases-shield]: https://img.shields.io/github/release/leikoilja/ha-google-home.svg?style=for-the-badge
[releases]: https://github.com/leikoilja/ha-google-home/releases
[workflow-shield]: https://img.shields.io/github/workflow/status/leikoilja/ha-google-home/Running%20tests?style=for-the-badge
[workflow]: https://github.com/leikoilja/ha-google-home/actions
