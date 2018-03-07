# Electrical Engineering
+ [Resistors](#ee-resistors)
+ [Voltage Dividers](#ee-voltdiv)
+ [Kirchhoff's Laws](#ee-kirchhofflaws)
## Resistors <a id="ee-resistors"></a>
+ Resists/limits flow of electrons through a circuit
+ Measured in **ohms**
+ Series: sum of resistors
	+ `R`<sub>`total`</sub> `=` `R`<sub>`1`</sub> `+` `R`<sub>`2`</sub> `+ ... +` `R`<sub>`n`</sub>
+ Parallel: inverse of sum of inverses
	+ `R`<sub>`total`</sub> `=` (`R`<sub>`1`</sub><sup>`-1`</sup> `+` `R`<sub>`2`</sub><sup>`-1`</sup> `+ ... +` `R`<sub>`n`</sub><sup>`-1`</sup>)<sup>`-1`</sup>
+ Ohm's Law: `V = IR`
	+ `V` voltage (volts)
	+ `I` current (amperes)
+ Resistors dissipate power in the form of heat: `P = IV`
	+ `P` power (watts)
	+ `I` current
## Voltage Dividers <a id="ee-voltdiv"></a>
+ A type of resistor circuit that turns big voltages into smaller ones
+ Uses two resistors in series:

	![Diagram of a voltage divider](volt_div.png)
+ `V`<sub>`out`</sub> `=` `V`<sub>`in`</sub> `*` `R`<sub>`2`</sub> `/` `(R`<sub>`1`</sub> `+` `R`<sub>`2`</sub>`)`
+ `V`<sub>`out`</sub> is the smaller voltage
## Kirchhoff's Laws <a id="ee-kirchhofflaws"></a>
+ Kirchhof's Current Law (KCL): sum of currents flowing into a circuit element is equal to the sum of currents flowing out of that element
+ Kirchhof's Voltage Law (KVL): the sum of the voltage difference across all circuit elements (including source) is 0