# 1PPS TTL to RS232 level converter

> This work is inspired by "Gadget box" from Dave Mills presented in the ntp.org documentation section [Pulse-Per-Second (PPS) Signal Interfacing](https://www.ntp.org/documentation/4.2.8-series/pps/)


## Why
When working with some legacy ntp systems i came across a breadboard laying around to get a TTL 1PPS pulse from a GPS reciever into the system using a rs232 interface.
While this had been in "production" for a bunch of years and actually works, it is not by far "robust enough" solution in my mind.
After tracing down the original circuit i re-created it using a real pcb and a spare project box laying around.

## Logic
The board is centered around a 74LS123 Retriggerable monostable multivibrator that triggers on the TTL 1PPS pulse and have a adjustable hold-time.
This pulse is then feed into a MAX232 to make it a real rs232 signal level detectable by the rs232 interface on the system.

From the GPS/PPS source the PPS pulse is collected via a 50ohm coaxial cable terminated by BNC, this input contains placements for two resitors that can be fitted with 50ohm or 100k ohm resitors as needed for termination of the signal (R1 and R2).
PPS input is then feed into the first vibrator circuit(U1A) on the 74LS123. This input have a adjustable holding-time(RV1) to make it possible to use wery short signals from the GPS/PPS source on a system needing a longer detection time.

The inverted output from this vibrator is then feed into a transmitter of a MAX232 to get the desired RS232 signal levels needed and is feed out DB9-pin1 (Data Carrier Detect).

## Power
The device is possible to power two ways, either via the DTR pin or via a 6-12v power supply via the DC Barrel jack.
Some systems does not have enough power to drive the board via the DTR pin, so a diode(D2) is added that optionally can be placed to enable this.

# Test-points and delay measurements
| TP  | Signal                               | Signal Level |
| TP1 | PPS input signal from GPS/PPS source | TTL          |
| TP2 | Signal between 74LS123 and MAX232    | TTL          |
| TP3 | Output from PPS Signal LED           | TTL          |
| TP4 | Output from MAX232                   | RS232        |

Calculation of the  device's delay can be done by using a oscilloscope or logic analycer to measure the difference in time between changes on TP1 and TP4.
