# home-automation
Home automation system.

TO DO: Add home assistant scripts for the relevant parts.

This is a system for home automation which use control systems to regulate a set of parameters. It integrates with pomodoro method by setting the light colour for environmental cues. The system consists of two parts - the control subsystem and the pomodoro subsystem.

## Control system
This system measures and adjusts 4 parameters: CO2 concentration, temperature, humidity and particulate matter pm2.5. There is a sensor that measures each of these parameters.

Here are the mechanisms that adjust these parameters:
* open the window to lower CO2
* turn on the heater to increase temperature
* close the window to increase temperature
* turn on humidifier to increase humidity
* close the window to increase humidity
* turn on air purifier to decrease particulate matter

As can be seen from the list, this system is location dependent as makes some assumptions that may not hold everywhere, such as that air outoors is less humid. Parameters are changeable only in one direction, which is fine for CO2 and particulate matter, but for humidity and temperature it depends on the environment.

The system checks the sensors every 5 minutes and makes appropriate adjustments. A smart light bulb is used to notify the residents if the control system fails to containt the parameters within healthy values. The data is stored for analysis, which allows for searching patterns. One example would be integrating Oura ring sleep tracker and determining whether the sleep quality is influenced by air quality and temperature.

More precisely, this cycle is evaluated iteratively unless no one is at home:
1) If CO2 ppm>1600, open the window and switch the smart light bulb to red.
2) If temperature is less than 22 degrees Celsius, turn on the heater. If it is less than 15 degrees and the window is open, close it; also switch the smart bulb to red.
3) If the relative humidity is below 30%, turn on the humidifier. If it is below 20%, close the window it is open; also switch the smart bulb to red.
4) Measure the pm 2.5 in micrograms per cubicmeter.
If it is less than 12, set the air purifier to speed 1.
If it is between 12 and 35, set the air purifier to speed 2.
If it is between 36 and 55, set the air purifier to speed 3.
If it is more than 55, set the air purifier to speed 4. Also switch the smart bulb to red. This might happen do to the air pollution outside (in which case close the window if it is open) or from indoors. Indoor sources of air pollution can occur as a result of cooking, incense or other sources of fine particles. It is recommendable to mitigate the pollution or turn off the air purifier depending on which cause is the most common one in the concrete implementation.
5) If all parameters are within the norm and the smart bulb is still red, switch it to warm white.
6) Collect the measurements and save it for later analysis.
7) If the iteration is completed and 5 minutes have passed since the beginning of the cycle, start the next iteration - go to step 1. 

## Pomodoro environment
The pomodoro technique is a time management method where a timer is set for 25 minutes where one works on something productive. Then a 5 minute break is held before repeating the cycle.

This system assists in performing pomodoro technique by providing environmental cues with colour changes. In the active phase of the pomodoro the smart bulb is switched to blueish white. This provides cues to initiate the habit of doing work, also blue light has been shown to decrease the production of melatonin, which should decrease sleepiness and improve focus.

In this system once the user initiates the pomodoros, the light is switched to blueish white and after 25 minutes the user is queried whether they want to continue and whether to take a break. If the light is turned red due to the abnormalities in the regulated parameters, the pomodoro is cancelled and the light is switched to red, announcing the problem to the user. The completed pomodoros are saved.
