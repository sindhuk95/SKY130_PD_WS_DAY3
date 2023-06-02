# SKY130_PD_WS_DAY3
# Design Library cell

 - PnR is a iterative flow and hence, we can make changes to the environment variables in the fly to observe the changes in our design. 
 - Let us say If I want to change my pin configuration along the core from equvi distance randomly placed to someother placement, we just set that IO mode variable on command prompt as shown below
 ```
 set ::env(FP_IO_MODE) 2
```

# SPICE Deck and Simulation

### SPICE Deck or Netlist creation for CMOS inverter 
- Before creating a SPICE simulation we need to create SPICE Deck
SPICE Deck provides information about the following:
- Component connectivity - Connectivity of the Vdd, Vss,Vin, substrate. Substrate tunes the threshold voltage of the MOS.
- component values - values of PMOS and NMOS, Output load, Input Gate Voltage, supply voltage.
- Node Identification and naming - Nodes are required to define the SPICE Netlist
     For example ```M1 out in vdd vdd pmos w = 0.375u L = 0.25u``` , ```cload out 0 10f```
- Simulation commands
- Model file - information of parameters related to transistors

Simulation of CMOS using different width and lengths. From the waveform, irrespective of switching the shape of it are almost same.

- ![image](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/5b494ae5-341a-41ff-a2bb-1db066fa2b72)

From the waveform we can see the characteristics are maintained  across all sizes of CMOS. So CMOS as a circuit is a robust device hence used in designing of logic gates. Parameters that define the robustness of the CMOS are

### Switching Threshold Vm
- The Switching Threshold of a CMOS inverter is the point where the Vin = Vout on the DC Transfer characreristics. 
- At this point, both the transistors are in saturation region, means both are turned on and have high chances of current flowing driectly from VDD to Ground called Leakage current.
![Capture13](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/46960968-4974-451a-ae0f-fa0a158a739a) 

![Capture14](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/1f21aea7-aa0c-4ace-8aa4-d381cb134644)

Through transient analysis, we calculate the rise and fall delays of the CMOS by SPICE Simulation. As we know delays are calculated at 50% of the final values.

![image](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/f4b115e6-bd2f-419e-b457-d1cdd5dd38dd)

### Mask CMOS Fabrication

The 16-mask CMOS process consists of the following steps:

- #### Selecting the subtrate
- #### Creating actove region for Transistors #### -  Isolation between active region pockets by depositing the oxide layer SiO2, silicon nitride Si3N4,then photoresist and masking the layers followed by photolithography and etch of the silicon nitride and remove photoresist. then this is placed in oxidation furnace, helps in growing oxide layer in other regions. This process is called LOCOS "Local oxidation of silicon" and lastly we strip out Si3N4 using hot phosphoric acid.
- #### N-well and P-well formation: Both wells are created separately. First Pwell is created in steps like Photoresist, mask, photolithpgraphy and a p-type Boron material is diffused into psubstrate using Ion Implantation, forming P-well and same goes for n-well formation by diffusing a n-type Phosphorous material. Till now wells are created, by using High Temperature furnace process, drive-in dussion happens and  creating depths of wells and this process of creating is called tub process.
- #### Formation of gate - Gate is the most important terminal of CMOS transistor, where you control the threshold voltages and these voltages turns off/on the transistor.The two parameters for gate formation are
   - #### Oxide capacitance
   - #### doping concentration NMOS and PMOS gates formed by photolithography techniques.
- #### LDD (lightly doped drain) formation: LDD formed to prevent hot electron effect.
- #### Source & drain formation: Screen oxide added to avoid channelling during implants followed by Aresenic implantation and annealing.
- #### Local interconnect formation: Removal of screen oxide by HF etching. Deposition of Ti for low resistant contacts.
- #### Higher level metal formation: CMP for planarization followed by TiN and Tungsten deposition. Top SiN layer for chip protection.





