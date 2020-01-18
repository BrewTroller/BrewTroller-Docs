BrewTroller 101
BrewTroller 101 is your guide to understanding, planning, building, and running your BrewTroller.  We have tried to design it in such a way that someone with no experiance can get the guidance needed to get started and be successful!

If there is a topic or item you think is missing, let us know!

Our recommendation is to go through these documents in order:

Decision Guide
Buyer's Guide
Parts List
Getting Started
These other documents will help you decide on different layouts and setups, understand accessories, and a lot more info.  Check them out as well!

Add On Modules
M12 One Wire Network
Volume Measurement
And, if you have already purchased a BrewTroller Phoenix, the Quick Start Guide is a great place to start!

Getting Started
BrewTroller is a firmware application for monitoring and controlling your brewing system. The firmware is designed to run on the BrewTroller line of controllers developed and manufactured BrewTroller. Legacy "BrewTroller 3.x" and "BrewTroller 4.x" controllers manufactured by OSCYSYS are also still supported by the firmware at this time.

Temperature Monitoring and Control
BrewTroller uses Maxim DS18B20 1-Wire digital temperature sensors to monitor various temperatures in your brewing system. These sensors connect via a 1-Wire communication bus that uses a single data wire along with signal ground and bus power. The use of 1-Wire temperature sensors allows many temperature sensors to be connected to the system. Currently BrewTroller supports nine temperature sensors but additional sensors could easily be added in future releases. The supported sensors are: HLT, Mash, Kettle, H2O In, H2O Out, Wort Out, AUX1, AUX2 and AUX3. The auxiliary outputs can be used to monitor custom components in a user's system. A Mash Averaging features is also available allowing multiple sensors in the mash tun to be averaged and the resulting value used to control the Mash heat logic. 

BrewTroller supports up to four heat outputs for temperature control. Outputs that are not dedicated to temperature control can be used to control pumps and valves. The typical heat output roles are HLT, Mash, Kettle. The Hardware Profiles section further discusses how heat outputs can be assigned.

Each heat output can be configured to use On/Off or PID Mode. On/Off control simply turns off an output if the setpoint has been reached and turns on the output if the temperature falls below the setpoint. A hysteresis value is used to create a deadband and avoid rapid cycling of the output. The hysteresis will cause the output to remain off unless the value drops below the setpoint by more than the hysteresis value.

In PID mode the output operates with Pulse Width Modulation (PWM). You define the cycle time for the output (for example 1s) and the PID controller will control the duty cycle between 0-100%. At 75% power for a 1s cycle time the output will be turned on for 750ms and then off for 250ms, repeatedly. This functionality was designed to be used with electric heating elements in vessels and provides fine control of vessel heat.

Smart Boil Control
BrewTroller provides advanced boil control logic that is useful for both electric and gas boil kettles. Two user-defined settings control the boil logic. The first setting, Boil Temp, applies to both gas and electric and defines what the boiling temperature is which varies based on altitude. The second setting, Boil Power, is used for electric kettles using PID Output Mode.
 
Boil Control for Electric Boil Kettles
When using BrewTroller with Boil Kettles equipped with electric heating elements it is recommended the Kettle Heat Output be configured in PID Mode. When the Boil Step is started BrewTroller activates the Kettle output at full power. However, once the Boil Temp value has been reached the output is reduced to the user-defined Boil Power value. This Boil Power value is the value required to maintain the boil.
Whenever the BrewTroller UI is locked to the Boil Screen (occurs automatically when advancing from Sparge to Boil) the Encoder or Control Knob directly controls the Kettle Output. This allows the user to quickly intervene to reduce boil power to prevent a boil over. Boil Power and Boil Temp values can be set directly from the Boil Menu during program execution. This might be helpful as a brewer fine tunes their system.
 
Boil Control for Gas Boil Kettles
Gas kettles with a single On/Off valve will generally lack the fine control needed to quickly reach and then maintain a boil without user intervention. However, more advanced gas systems can take advantage of the Kettle Heat and Kettle Idle valve profiles to provide additional control. In this configuration multiple gas lines each with their own solenoid valve and regulator would be connected to the same burner. One regulator would be set at the output value used to bring the kettle to boil and the valve that feeds this regulator would be enabled with the Kettle Heat valve profile. The other regulator would be set at a lower value used to maintain boil and activated with the Kettle Idle valve profile. Neither valve would be directly controlled by the Kettle output.
When the Boil Step is started it sets the Kettle setpoint to the user-defined Boil Temp. Whenever a vessel's setpoint is active the valve control logic will enable either the Kettle Heat or the Kettle Idle valve profile. Initially the Kettle Heat profile will be activated as the current temperature will be below Boil Temp. However, once the Boil Temp is reached the Kettle Idle profile will be enabled which can be used to maintain the boil. If the temperature falls below the Boil Temp by more than the user-defined Kettle Hysteresis value the Kettle Heat output will be reactivated. Fine tuning of the Hysteresis and regulator values will allow automatic control of the boil with little to no user intervention.

Smart HERMS HLT
BrewTroller also supports a unique Smart HERMS HLT feature. When this compile-time option is enabled the HLT setpoint will be automatically set during Preheat and mash steps according to the difference between the Mash setpoint and actual temperature. The HLT setpoint is set to the mash setpoint plus twice the difference of the mash setpoint/actual up to a user-defined maximum HLT setpoint value. As the mash gets closer to target the HLT setpoint moves closer to that of the Mash to avoid overshooting the target temperature. An additional user-defined offset allows the HLT setpoint to be adjusted to account for heat loss in plumbing, etc.

Valve Profiles
Valve profiles act as an abstraction layer between BrewTroller's process control logic and a brewer's system. BrewTroller can simply activate the HLT Fill profile and the brewer defines what outputs need to be enabled by that profile to perform a specific function. The other advantage of using valve profiles is the ability to easily use devices for multiple functions in the brewing process. For example, one profile might one a valve from the Mash Tun to a pump, open a valve from the pump to a HERMS coil that returns to the Mash and activate the pump. Later in the process the same mash to pump and pump outputs may be used but with a different output valve enabled on the pump to direct mash liquor to the kettle. Valve profiles allow the same BrewTroller code to work for dramatically different systems without requiring custom code.

Volume Measurement
BrewTroller controllers include analog inputs that can be used for measuring vessel volume. BrewTroller uses a value map that correlates specific DC voltages (0-5v DC) to specific volumes. The most reliable method we've found for producing this kind of signal is using a pressure transducer like the Freescale MPX5010 attached to a small pick up tube at the bottom of the vessel. The pressure produced by a volume of liquid in a vessel is directly related to the height of the liquid in the vessel. This specific sensor can produce a 0-5v signal that corresponds with 0-40" H2O which makes it well suited for most homebrewing applications. Freescale offers pressure sensors in various ranges allowing another sensor to be adopted to larger vessel sizes.

When using volume measurement with BrewTroller you must first calibrate the sensor. Each vessel map has ten slots to record calibration data. You fill your vessel to a known volume and set a calibration. BrewTroller maps the current voltage output of the sensor to the volume you specify. When calculating the volume of a vessel BrewTroller will use the two best calibration points relative to the current sensor voltage, usually one above and one below. BrewTroller will calculate any value between the two calibration points in a linear fashion.

One issue we've found with using pressure sensors is the impact from temperature changes, specifically when temperature decreases. When temperatures increase the pressure builds and excess air bleeds out of the dip tube. When the temperature decreases the pressure reduces creating a vacuum that affects volume measurements. The BrewTroller community developed a solution using a small air pump with needle valves for each vessel. By constantly feeding a small supply of the air a vacuum cannot develop. Excess pressure will bubble out of the diptube. Using this method accurate volume measurements across temperature changes is possible.

Hardware Profiles
Hardware profiles are used within the BrewTroller code to support various vessel and heat output configurations. Hardware profiles allow the same BrewTroller code to be applied to different controllers and further defines the number and role of specific heat outputs. A number of predefined hardware profiles are available for each controller. Advanced users may modify the hardware profile as needed to support customization.

Direct Heat/RIMS: This configuration uses three heat outputs, one each for the HLT, Mash and Kettle. Use this profile for a three vessel direct-fired gas system, All-Electric RIMS or any hybrid combination.
HERMS: This configuration uses two heat outputs, one for the HLT and one for the Kettle. A dedicated heat output is not required for the Mash. A pump and optional valves can be controlled using the Mash Heat/Mash Idle valve profiles. Using a valve profile for this function allows the same pump to be used for other functions within the brewing process.
Single Vessel: This configuration uses a single heat output for a single vessel designed for no-sparge brewing.
Four Vessel: This configuration (currently named Steam/PWM Pump) is similar to the Direct Heat/RIMS configuration but provides a fourth output that can dedicated to special functions. For example, a special compile option called "Direct-Fired RIMS" allows the mash tun to be heated directly (usually with a gas burner) for large temperature steps and switches to a RIMS element for finer control when the temperature is within a specific range of the setpoint. Another compile option allows flowrate control using a PWM pump. Finally a third option for this profile is the use of a dedicated Heat Exchange vessel for a HERMS system.
User Interface
A 20x4 character LCD and a three-function (left/right/click) encoder serve as the user interface. The BrewTroller UI is arranged in a series of screens that are specific to the various steps in the brewing process. When the screen is locked the encoder controls elements on that screen. A long click (> 1s) unlocks the UI allowing you to scroll to another screen. A short click locks the UI on the current screen shown. [Example Screens]

The PID Display add-on module adds LED segment display(s) to your system in a two-line format common on standalone PID controllers. The PID Display module is assigned to a specific function. Standard functions include a specific vessel's actual and target temperature or volume. A Mash/Boil timer function is also available. Advanced users can write custom functions by copying or modifying a built-in function.



Workflow/Recipes
Most brewers are attracted to BrewTroller's for its workflow functionality. A brewer can define a recipe (referred to as a "program") prior to starting their brew day and then start that program to begin brewing. BrewTroller steps the brewer through that brew session setting setpoints automatically, activating timers when setpoints are reached and sounding alarms when user interaction is required. Up to 20 programs can be saved in the controller's memory allowing a recipe to be repeated with the same parameters. A user-defined name for each program makes it easy to identify. The following settings define a program:

Batch Volume: The total volume expected in the kettle at the end of the brewing session. Volume loss due to hops and hot break material is not included in calculations. You should increase your batch volume accordingly. For example, if your recipe assumes 5.5 gal will be collected in the fermentation tank and 0.5 gal will be lost in the kettle due to hop/break loss you should specify 6 gal for this setting.
Grain Weight: The total weight of grain in the mash. This is used with the mash Ratio setting to calculate strike volume.
Boil Length: The duration of the boil. This is used with the user-defined system evaporation rate to calculate pre-boil volume.
Mash Ratio: A value specified as (qts/lb or l/kg) used to calculate strike volume. A "No Sparge" option instructs the system to use the entire preboil volume (plus grain losses) for the strike volume.
HLT Tempurature: The setpoint for the HLT during Preheat and Mash Steps
Sparge Temperature: The setpoint for the HLT used during the End Mash step prior to starting the Sparge.
Pitch Temperature: The target temperature for wort in the fermetation tank. This value will be used in a future release to support automatic chilling/whirlpool logic in the kettle.
Mash Schedule: Specify the temperature and duration of each mash step. A step with a zero temperature will be skipped. A step with a temperature and a zero duration will continue to the next step as soon as the temperature is reached (typically used for final Mash Out step).
Boil Additions: A schedule indicating when boil additions should occur. The options are: Boil, 105, 90, 75, 60, 45, 30, 20, 15, 10, 5 and 0 Mins. An alarm will sound at each addition. A Boil Addition valve profile is also available to activate a hop dropper for automated additions.
Strike Heat Source: Defines where strike water volume should be filled for Preheat. HLT and Mash and currently supported but Kettle is expected to be supported in a future release.
Brewing Calculations
BrewTroller automatically calculates:

Mash Liquor Volume
Sparge Volume
Strike Temperature
For many brewers this reduces their dependency on brewing other software after initial recipe formulation (calculating IBU, color, OG/FG/ABV, etc.). Factors used in these calculations (grain volume, grain absorbtion, etc.) can be adjusted by the brewer. Currently these values are adjusted at compile-time but will be moved into System Setup within the BrewTroller UI in a future release.

Network Interface
Network connectivity is available integrated (BrewTroller DX2), optionally integrated (BrewTroller BX2) or as an add-on Ethernet module (not available yet). This module provides a web service that can be used to monitor and control BrewTroller. Currently a web-based UI is not hosted on the module directly. Instead, a free web UI called BrewTroller Web App developed by BrewTroller is hosted by us. Although the BrewTroller Live is hosted on the internet the communication to the network interface occurs directly from the device where the web browser is running via asynchronous AJAX calls. Therefore the network interface does not need to be exposed directly to the internet unless you are trying to connect from outside your internal network.

Technical users can develop their own custom interfaces or write custom scripts to provide their own alerting, configuration, backup/restore, and other functionality.


