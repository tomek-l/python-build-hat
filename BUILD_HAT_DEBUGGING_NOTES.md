
The more "technical" documentation is here: http://www.raspberrypi.com/documentation/accessories/build-hat.html


Possible root causes:
- Power Issue
	+ Context: The RED LED on build hat *sometimes* turns off when connected to Jetson.  The issue never occurs when the buildhat is connected to Raspberry Pi
	+ Hypothesis: The collective power draw of causes a black-out/brown-out on RP2040 chip, but not on Jetson Nano chip.
	+ 5V and 3.3V lines seem healthy `(5.006V and 3.330V respectively)`, even when powering Jetson
	+ The RED LED doesn't extingiush consistently (sometimes I'm able to power Jetson Nano with buildhat and the LED doesn't extinguish)
	+ __Resolution:__ Order the dedicated PSU to eliminate this as possible root cause.

- Connectivity issue
	+ Hypothesis: the 40-pin connector doesn't make a proper connection with Jetson header making serial connection impossible
	+ __Next steps:__ probe the RX/TX lanes with logic analyser, make sure we're getting a healthy signal with correct logic levels.

- UART misconfiguration issue
	+ My assumption is that `/dev/ttyTHS1` is *always* mapped to pins 8 and 9 on the GPIO output (which connect to buildhat). Could that be not the case?
	+ __Next steps:__ probe UART TX with a logic analyser during serial.write() calls. This can be resolved jointly with addressing connectivity issue.

