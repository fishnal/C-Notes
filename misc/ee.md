# Electrical Engineering

+ [Voltage](#ee-voltage)
+ [Resistors](#ee-resistors)
+ [Voltage Dividers](#ee-voltdiv)
+ [Kirchhoff's Laws](#ee-kirchhofflaws)
+ [Admittance](#ee-admittance)

## Voltage <a name="ee-voltage"></a>

+ Voltage drops are completely relative
+ You don't find voltage "at a point", you find the voltage drop between two points/locations on the circuit
+ Input voltage doesn't equate to a source

## Resistors <a name="ee-resistors"></a>

+ Resists/limits flow of electrons through a circuit
+ Type of impedance
+ Measured in **ohms**
+ Series: sum of resistors
	+ R<sub>total</sub> = R<sub>1</sub> + R<sub>2</sub> + ... + R<sub>n</sub>
+ Parallel: inverse of sum of inverses
	+ R<sub>total</sub> = (R<sub>1</sub><sup>-1</sup> + R<sub>2</sub><sup>-1</sup> + ... + R<sub>n</sub><sup>-1</sup>)<sup>-1</sup>
+ Ohm's Law: V = IR
	+ V voltage drop (volts)
	+ I current (amperes)
	+ Just because there may be no current flowing through a circuit, there is still a voltage
		+ The concept of voltage drop comes back here: there's no voltage drop across our no-current circuit, but there still is a voltage
	+ Remember that the voltage value we're using is the voltage *drop*, not necessarily the voltage "going in"
+ Resistors dissipate power in the form of heat: P = IV
	+ P power (watts)
	+ I current

## Voltage Dividers <a name="ee-voltdiv"></a>

+ A type of resistor circuit that turns big voltages into smaller ones
+ Uses two resistors in series:

	![Diagram of a voltage divider](ee_volt_div.png)
+ V<sub>out</sub> = V<sub>in</sub> * R<sub>2</sub> / (R<sub>1</sub> + R<sub>2</sub>)
+ V<sub>out</sub> is the smaller voltage

## Kirchhoff's Laws <a name="ee-kirchhofflaws"></a>

+ Kirchhof's Current Law (KCL): sum of currents flowing into a circuit element is equal to the sum of currents flowing out of that element
+ Kirchhof's Voltage Law (KVL): the sum of the voltage difference across all circuit elements (including source) is 0

## Admittance <a name="ee-admittance"></a>

+ One type of admittance is **mho**, the inverse of resistance
	+ Symbol is upside down omega