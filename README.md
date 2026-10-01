# GMOS LIA Readout Board
This is a Kicad project implementing the PCB design of the GMOS readout using the Lock in amplifier (LIA) approach.

# Schematic Overview
Three main pages:
1.
    a. Connectors
    b. GMOS socket
2. Analog readout section
3. Digital readout
4. DC section

## DC

## Requirements
In order for the board to be as portable as possible, and not to require too ports for power sources, its therefore required to impolement some DC circuits on the board to generate the required voltage rails for the board. The general scheme that is planned is as follows:

> Add a block diagram of the DC system, using mermaid
> VIN (from the power input connector) applies to two chains of blocks, top chain is for the positive rails, lower for negative rail. First block in each chain is a buck converter (for the positive its connected regularly, bottom its connected in a IBB topology, according to #(Working With Inverting Buck-Boost Converters, TI application note)[docs/snva866b.pdf]. The positive buck output goes in parallel to two linear regulators, and the IBB goes to a single negative voltage linear regulator.
1. Input voltage that is required to be supported is 11-15V
2. Intermiddiate buck and IBB will generate approximately 7V for reduction of power dissipation on the follow-up linear regulators
3. Maximal ripple for buck and IBB is 10% of voltage
4. linear regulators for analog rails will regulate for 5V and -5V
5. An additional positive voltage of 3.3V will be generated for digital circuitry
6. current draw requirements from the analog circuits can be calculated from the [analog section requirements](#Analog section)


### Positive Buck
Used to generate the intermiddiate positive rail that acts as the input for the 5V analog and 3.3V digital voltage rails
> choose component according to listed requirements, then choose peripherial components with respect to their tolerance power, DC bias and availability
> Include calculations for feedback resistor network, and inductor selection


## Inverting Buck
In order for us to generate the negative rail for the analog circuitry 
> choose component according to listed requirements, then choose peripherial components with respect to their tolerance power, DC bias and availability

## Linear Regulators
Used to generate the stable voltage rails for the analog circuits as well as the digital and voltage converters.
> choose component according to listed requirements, then choose peripherial components with respect to their tolerance power, DC bias and availability

## Analog Section
The analog section of the LIA board is used for:
1. Controlling the gate voltage of the GMOS transistors
3. Voltage applied to the drain of the GMOS
4. Amplification and AC filtration of the differential voltage of the GMOS bridge
5. Extraction of the inphase and quadrature components of the amplified voltage using the ad630.
6. DAC component will control the DC components of the gate voltage
7. DAC and amplifier that will control the voltage of the heaters DC voltage - the range required is 2.5V up to 4V with resolution of 12bits at least. 
8. ADC (with excellent accuracy and resolution but slow enough interface that can be read with simple MCU) for the readout of the X and Y channels of the GMOS analog output
9. ADC with resolution good enough to measure current with resolution of 1mA on two heater resistors. the heaters have resistance of 600-1.2Kohm
10. DAC and amplifier will control the voltage of the gmos drain, it will be connected through a series resistance to limit the current (resistor of appx 330Kohm for example) voltage accuracy required is 10uV and current measuremet is also required with accuracy of 1uA. Voltage range to be applied is 2.7-3.5V.
11. Each GMOS package has two usable differential channels (bling-active pair) so in order to not implement the differential signals readout twice a switch component (prefferably analog switch, if not possible, then analog one) will be used so that the analog chain of amplification/filtration and the ad630 pair of readout will be implemented once
12. frequency of 518Hz will be generated using a quadrature oscillator using op-amp pair, the gate voltage will be the output of a summation opamp of the sine with a DC component from the gate DC DAC. sine and cos will go to the comparator inputs of the ad630.
13. the ad630 will be connected in a lock in amplifier topology with amplification of 1
14. amplification chain priore to the LIA inputs must implement 20dB amplification in 518Hz with BW of 100Hz, with LPF and HPF filters with 6dB slope on each side. implemented using Sallen-key filter HPF topology and RC LPF for each drain, then the result is fed into instrumentation amplifier and into the LIA.
15. 5V rail will also power the ESD protection circuitry (appx 5mA draw)

> need to estimate the current draw for the component selection for power circuits.

## Digital Section
The digital section must include
1. MCU that can control the DACs and ADCs for the analog section control and readout
2. digital temperature sensor
3. digital humidity sensor
4. connection to serial port for communication with PC
5. JTAG connector for flashing the MCU

