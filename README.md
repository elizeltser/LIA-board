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

> Add a block diagram of the DC system, using mermaid
> VIN (from the connector) comes from the left, applies to two blocks ("in parallel" just to the right of VIN). one block is 7V Buck converter (LT8610) the other is -6.8V inverter charge pump (LT3483). From the top block (LT8610) their output goes to yet again two blocks in parallel, both of which are ADP7118, one is set to 3.3V other to 5V. Finally from the -6.7V line, there is another block with output -5 that is a negative regulator LT3094.

### 7V Buck
Here we use LT8610, the different component selections
1. R2 and R3 selected to set 7V being the output, R2=5.1k R3=820, so that Vout=0.97V(1+R2/R3)
2. C1 set to 100nF according to datasheet recomendation to BST
3. R1 chosen to be 33.2k so that switch frequency will be 1.2MHz
4. L1 inductor is initially set to 6.2uH which is closesed typical value that is higher than (Vout+Vsw)/fsw=(7+0.15)/1.2M. *still need to calculate expected ripple and from that the minimum saturation current)
5. C4 at least 1uF according to INTVCC requirements in datasheet
6. C2 set to 10pF for boost network feedback loop, should be DNP

## Linear Regulators
The components U4 and U5 are linear regulators used for both filtering out any switching noise whose source can be the input buck circuit.

The calculation for the Vfb:

Vout=1.2V(1+Rhigh/Rlow)
if soo, that must mean that 
1. For the opposition even worse.
