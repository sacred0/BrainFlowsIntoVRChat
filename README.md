# BFiVRC: BrainFlowsIntoVRChat

This is a BrainFlow implementation of my [bci-workshop fork](https://github.com/ChilloutCharles/bci-workshop) that sends your brain's relaxation and focus metrics, and power values based on the common frequency bands used in EEG measuements, for left, right and both sides of the head. Additional support for heart rate and respiration is available when supported by your hardware.

## Why BrainFlow?

The [BrainFlow](https://BrainFlow.org) library provides a uniform API that is device agnostic, allowing this implementation of my workshop fork to work for all [supported biosensors](https://BrainFlow.readthedocs.io/en/stable/SupportedBoards.html) (including the muse headbands). No extra software needed!

## Demos
- [Rantis's Brain Controlled Ears](https://twitter.com/RantiMess/status/1746704510972580061)
- [Brain Controlled Ears: Five Months Later [VRChat]](https://www.youtube.com/watch?v=kPPTT3ogEgg)
- [VRCHAT OSC MAGIC! (Last 30 seconds)](https://twitter.com/kentrl_z/status/1497020472046800897)
- [Old version of Brain Controlled Ears](https://www.youtube.com/watch?v=WjWc51xNgKg)

## Instructions

1. Download this project to a folder and remember its folder path
2. Install [Python 3.11.5](https://www.python.org/downloads/release/python-3115/)
3. Open the command prompt by typing searching cmd at the start menu
4. Navigate to the project's path within the command prompt. 
   - example: `cd "C:\Users\<YOUR USERNAME HERE>\Documents\GitHub\BrainFlowsIntoVRChat"` 
5. Execute this command to install needed depedencies: `pip install -r requirements.txt`
6. Look up your device's id: [Board IDs Page](https://brainflow.readthedocs.io/en/stable/UserAPI.html?highlight=MUSE_2016_BOARD#brainflow-board-shim)
7. Turn on your headband
8. Run the script `main.py` with your device id. The command for running with a [Muse 2 headband](https://choosemuse.com/muse-2/) would be: `python .\main.py --board-id 38`

## OSC Avatar Parameter Schema

Here are the various avatar parameters sent to VRChat. Neurofeedback scores range from -1 to 1 for signed floats, 0 to 1 for unsigned, with higher and lower values corresponding to higher and lower relax/focus scores. Depending on the board you're using, respiration data and battery level might be available. Power Band numbers are also sent per location as well, ranging from 0 to 1 averaging at 0.2. 

To use parameters within VRChat, write the parameter name as a path. For example, to get the left side alpha value, the parameter name would be:
- `Brainflow/PwrBands/Left/Alpha`

```yaml
BFI:
  Info:
    - VersionMajor [int]
    - VersionMinor [int]
    - SecondsSinceLastUpdate [float]
    - DeviceConnected [bool]
    - BatterySupported [bool]
    - BatteryLevel [float]
  NeuroFB:
    - FocusLeft [float]
    - FocusLeftPos [float]
    - FocusRight [float]
    - FocusRightPos [float]
    - FocusAvg [float]
    - FocusAvgPos [float]
    - RelaxLeft [float]
    - RelaxLeftPos [float]
    - RelaxRight [float]
    - RelaxRightPos [float]
    - RelaxAvg [float]
    - RelaxAvgPos [float]
  PwrBand:
    Left:
      - Alpha [float]
      - Beta [float]
      - Theta [float]
      - Delta [float]
      - Gamma [float]
    Right:
      - Alpha [float]
      - Beta [float]
      - Theta [float]
      - Delta [float]
      - Gamma [float]
    Avg:
      - Alpha [float]
      - Beta [float]
      - Theta [float]
      - Delta [float]
      - Gamma [float]
  Addons:
    - Hueshift [float 0-1]
  Biometrics:
    - Supported [bool]
    - HeartBeatsPerSecond [float]
    - HeartBeatsPerMinute [int]
    - OxygenPercent [float]
    - BreathsPerSecond [float]
    - BreathsPerMinute [int]
```

## Depracation

We recommend updating to this schema. However, if your assets are still using the old parameter scheme, you can switch to them by adding the `--use-old-reporter` launch argument. Be aware that this will be deprecated in the future and an official announcement will be made for its sunset.

## Migration from Old Parameter Schema

Need to migrate your existing prefabs? You can convert your existing parameters to the new standard using this chart.

| Old Parameter | New Parameter |
| ------------- | ----------------- |
| osc_focus_left | BFI/NeuroFB/FocusLeft |
| osc_focus_right | BFI/NeuroFB/FocusRight |
| osc_focus_avg | BFI/NeuroFB/FocusAvg |
| osc_relax_left | BFI/NeuroFB/RelaxLeft |
| osc_relax_right | BFI/NeuroFB/RelaxRight |
| osc_relax_avg | BFI/NeuroFB/RelaxAvg |
| osc_heart_bpm | BFI/Biometrics/HeartBeatsPerMinute |
| osc_heart_bps | BFI/Biometrics/HeartBeatsPerSecond |
| osc_oxygen_percent | BFI/Biometrics/OxygenPercent |
| osc_respiration_bpm | BFI/Biometrics/BreathsPerMinute |
| osc_band_power_left_alpha | BFI/PwrBand/Left/Alpha |
| osc_band_power_left_beta | BFI/PwrBand/Left/Beta |
| osc_band_power_left_theta | BFI/PwrBand/Left/Theta |
| osc_band_power_left_delta | BFI/PwrBand/Left/Delta |
| osc_band_power_left_gamma | BFI/PwrBand/Left/Gamma |
| osc_band_power_right_alpha | BFI/PwrBand/Right/Alpha |
| osc_band_power_right_beta | BFI/PwrBand/Right/Beta |
| osc_band_power_right_theta | BFI/PwrBand/Right/Theta |
| osc_band_power_right_delta | BFI/PwrBand/Right/Delta |
| osc_band_power_right_gamma | BFI/PwrBand/Right/Gamma |
| osc_band_power_avg_alpha | BFI/PwrBand/Avg/Alpha |
| osc_band_power_avg_beta | BFI/PwrBand/Avg/Beta |
| osc_band_power_avg_theta | BFI/PwrBand/Avg/Theta |
| osc_band_power_avg_delta | BFI/PwrBand/Avg/Delta |
| osc_band_power_avg_gamma | BFI/PwrBand/Avg/Gamma |
| osc_battery_lvl | BFI/Info/BatteryLevel |
| osc_is_connected | BFI/Info/DeviceConnected |
| osc_time_diff | BFI/Info/SecondsSinceLastUpdate |
| HueShift | BFI/Addons/HueShift |

## Parameter Descriptions
### Utility
These utility Parameters give basic information about your device and BFiVRC
| Parameter | Description | Type |
| ------ | ----- | ----- |
| BFI/Info/VersionMajor | The major version number of current parameter schema | Int |
| BFI/Info/VersionMinor | The minor version number of current parameter schema | Int |
| BFI/Info/SecondsSinceLastUpdate | The refresh rate of BFiVRCs data stream | Float |
| BFI/Info/DeviceConnected | The connection status of your device to BFiVRC | Bool |
| BFI/Info/BatterySupported | If your device supports sending battery status to BFiVRC | Bool |
| BFI/Info/BatteryLevel | The current charge status of your devices battery | Float |

### Neurofeedback
These Parameters are calculated based on your current mental state and make use of the full signed positive and negative float range.
| Parameter | Description | Type | Range |
| ------ | ----- | ----- | ----- |
| BFI/NeuroFB/FocusLeft | Left Unfocused to Focused | Float | [-1.0, 1.0] |
| BFI/NeuroFB/FocusRight | Right Unfocused to Focused | Float | [-1.0, 1.0] |
| BFI/NeuroFB/FocusAvg | Unfocused to Focused | Float | [-1.0, 1.0] |
| BFI/NeuroFB/RelaxLeft | Left Excited to Relaxed | Float | [-1.0, 1.0] |
| BFI/NeuroFB/RelaxRight | Right Excited to Relaxed | Float | [-1.0, 1.0] |
| BFI/NeuroFB/RelaxAvg | Excited to Relaxed | Float | [-1.0, 1.0] |

These are the same Neurofeedback scores remapped to the positive 0 to 1 range for cases where it may be required.
| Parameter | Description | Type | Range |
| ------ | ----- | ----- | ----- |
| BFI/NeuroFB/FocusLeftPos | Left Unfocused to Focused | Float | [0.0, 1.0] |
| BFI/NeuroFB/FocusRightPos | Right Unfocused to Focused | Float | [0.0, 1.0] |
| BFI/NeuroFB/FocusAvgPos | Unfocused to Focused | Float | [0.0, 1.0] |
| BFI/NeuroFB/RelaxLeftPos | Left Excited to Relaxed | Float | [0.0, 1.0] |
| BFI/NeuroFB/RelaxRightPos | Right Excited to Relaxed | Float | [0.0, 1.0] |
| BFI/NeuroFB/RelaxAvgPos | Excited to Relaxed | Float | [0.0, 1.0] |

### PowerBand
These Parameters give the power value for the common frequency bands used in EEG measurements, measured per location. For more information on what each power band means, read more about it here: [What are Brainwaves](https://www.sciencedirect.com/topics/agricultural-and-biological-sciences/brain-waves)
| Parameter | Description | Type | Range |
| ------ | ----- | ----- | ----- |
| BFI/PwrBand/Left/Alpha | Left brainwaves Alpha band | Float | [0.0, 1.0] |
| BFI/PwrBand/Right/Alpha | Right brainwaves Alpha band | Float | [0.0, 1.0] |
| BFI/PwrBand/Avg/Alpha | Brainwaves Alpha band | Float | [0.0, 1.0] |
| BFI/PwrBand/Left/Beta | Left brainwaves Beta band | Float | [0.0, 1.0] |
| BFI/PwrBand/Right/Beta | Right brainwaves Beta band | Float | [0.0, 1.0] |
| BFI/PwrBand/Avg/Beta | Brainwaves Beta band | Float | [0.0, 1.0] |
| BFI/PwrBand/Left/Theta | Left brainwaves Theta band | Float | [0.0, 1.0] |
| BFI/PwrBand/Right/Theta | Right brainwaves Theta band | Float | [0.0, 1.0] |
| BFI/PwrBand/Avg/Theta | Brainwaves Theta band | Float | [0.0, 1.0] |
| BFI/PwrBand/Left/Delta | Left brainwaves Delta band | Float | [0.0, 1.0] |
| BFI/PwrBand/Right/Delta | Right brainwaves Delta band | Float | [0.0, 1.0] |
| BFI/PwrBand/Avg/Delta | Brainwaves Delta band | Float | [0.0, 1.0] |
| BFI/PwrBand/Left/Gamma | Left brainwaves Gamma band | Float | [0.0, 1.0] |
| BFI/PwrBand/Right/Gamma | Right brainwaves Gamma band | Float | [0.0, 1.0] |
| BFI/PwrBand/Avg/Gamma | Brainwaves Gamma band | Float | [0.0, 1.0] |

### Addons
These Parameters are additional functions of BFiVRC
| Parameter | Description | Type | Range |
| ------ | ----- | ----- | ----- |
| BFI/Addons/Hueshift | This Parameter uses combinations of relax/focus to drive a single float Parameter for material fx | Float | [0.0, 1.0] |

### Biometrics
These Parameters read other biometric data from your device if supported by your hardware
| Parameter | Description | Type | Range |
| ------ | ----- | ----- | ----- |
| BFI/Biometrics/Supported | If your hardware supports heart rate and respiration readouts | Bool | True/False |
| BFI/Biometrics/HeartBeatsPerSecond | Your heartrate calculated per second | Float | [0.0, inf) |
| BFI/Biometrics/HeartBeatsPerMinute | Your heartrate calculated per minute | Int | [0, 255] |
| BFI/Biometrics/OxygenPercent | Percentage of oxygen in blood | Float | [0.0, 1.0] |
| BFI/Biometrics/BreathsPerSecond | Estimated breaths taken per second  | Float | [0.0, inf) |
| BFI/Biometrics/BreathsPerMinute | Estimated breaths taken per minute  | Int | [0, 255] |


## Thanks

Thanks to 
- [@Mitzi_DelverVRC](https://twitter.com/Mitzi_DelverVRC) and [AartHark](https://github.com/AartHauk) for help with PPG signal work.
- [@wordweaver1001](https://twitter.com/wordweaver1001) for intial user testing.
- [AtriusX](https://github.com/AtriusX) for helping create a parameter schema.

## Troubleshooting
- I have broken bluetooth adapter built into my pc and I would like to use a dongle instead. How can I connect my headband to that dongle?
  1. Disconnect the bluetooth dongle you want to use
  2. Search up 'device manager' on the start menu
  3. Find an entry for a bluetooth radio, right click on it and disable it
  4. Plug the new bluetooth dongle back in

- Muse Headband connects just fine but times out after a few seconds. Solution: Reset the headband
  1. Turn off the headband
  2. Press and hold the power button until it turns on. Keep pressing until the light changes.
  3. Reconnect.

- I've set up everything and made a new avatar, but its still not reacting
  - Reason: VRChat stores cached OSC parameters for your avatar that aren't updated when the avatar is updated with new parameters
  - Solution: Go to `C:\Users\<YOUR USERNAME HERE>\AppData\LocalLow\VRChat\VRChat` and delete all folders under it, then reload avatar

## License
[MIT](http://opensource.org/licenses/MIT).
