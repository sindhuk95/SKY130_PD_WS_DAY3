# SKY130_PD_WS_DAY3
# Design Library cell using MAGIC Layout and ngspice characterization


# Table of Contents
1. CMOS inverter ngspice simulations
   - IO placer revision
   - SPICE Deck Creation and Simulation for CMOS inverter
   - Switching Threshold Vm
   - Lab steps to git clone vsdstdcelldesign
2. Inception of Layout and CMOS Fabrication Process
   - Mask CMOS Fabrication
   - SKY130 basic layer layout and LEF using inverter
3. Tech File Labs

# CMOS inverter ngspice simulations

### IO Placer revision
 - PnR is a iterative flow and hence, we can make changes to the environment variables in the fly to observe the changes in our design. 
 - Let us say If I want to change my pin configuration along the core from equvi distance randomly placed to someother placement, we just set that IO mode variable on command prompt as shown below
 ```
 set ::env(FP_IO_MODE) 2
```

### SPICE Deck Creation and Simulation for CMOS inverter

- Before performing a SPICE simulation we need to create SPICE Deck
SPICE Deck provides information about the following:
- Component connectivity - Connectivity of the Vdd, Vss,Vin, substrate. Substrate tunes the threshold voltage of the MOS.
- component values - values of PMOS and NMOS, Output load, Input Gate Voltage, supply voltage.
- Node Identification and naming - Nodes are required to define the SPICE Netlist
     For example ```M1 out in vdd vdd pmos w = 0.375u L = 0.25u``` , ```cload out 0 10f```
- Simulation commands
- Model file - information of parameters related to transistors

Simulation of CMOS using different width and lengths. From the waveform, irrespective of switching the shape of it are almost same.

- ![image](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/5b494ae5-341a-41ff-a2bb-1db066fa2b72)

From the waveform we can see the characteristics are maintained  across all sizes of CMOS. So CMOS as a circuit is a robust device hence use in designing of logic gates. Parameters that define the robustness of the CMOS are

### Switching Threshold Vm
- The Switching Threshold of a CMOS inverter is the point where the Vin = Vout on the DC Transfer characreristics. 
- At this point, both the transistors are in saturation region, means both are turned on and have high chances of current flowing driectly from VDD to Ground called Leakage current.
![Capture13](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/46960968-4974-451a-ae0f-fa0a158a739a) 

![Capture14](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/1f21aea7-aa0c-4ace-8aa4-d381cb134644)

Through transient analysis, we calculate the rise and fall delays of the CMOS by SPICE Simulation. As we know delays are calculated at 50% of the final values.

![image](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/f4b115e6-bd2f-419e-b457-d1cdd5dd38dd)

### Lab steps to git clone vsdstdcelldesign
- First, clone the required mag files and spicemodels of inverter,pmos and nmos sky130. The command to clone files from github link is:
```
git clone https://github.com/nickson-jose/vsdstdcelldesign.git
```
once I run this command, it will create ``vsdstdcelldesign`` folder in openlane directory. I have got an error saying `` could not  resolve host: github.com`` 
somehow I figured it was due incorrect proxy, so I used this command  `` git config --global --unset https:proxy``. I worked and could clone the files from github to my openlane.

![day3-1](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/5fcb599b-48bf-4667-995c-2ff99e7b6358)

The information of inverter layout is mentioned in ``sky130_inv.mag`` file.

![day3-7](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/d42a28e9-34a0-46c7-8799-03c668709207)

Inorder to open the mag file and run magic, we need tech file for magic in that location. So, lets copy the tech file from magic which is in pdks directory to vsdstdcelldesign folder
command to copy the file is

``` cp sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/ ```

once we run this command, the tech file will be copied to vsdstdcelldesign folder.

![image](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/172d04b0-5701-4bdd-b020-e32775932bf4)

For layout we run magic command

``` magic -T sky130A.tech sky130_inv.mag & ```

Ampersand at the end makes the next prompt line free, otherwise magic keeps the prompt line busy. Once we run the magic command we get the layout of the inverter in the magic window

![day3-3](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/47174505-3cbc-4110-b342-317eef4d849d)



# Inception of Layout and CMOS Fabrication Process

### Mask CMOS Fabrication

The 16-mask CMOS process consists of the following steps:

- Selecting the subtrate
- Creating actove region for Transistors -  Isolation between active region pockets by depositing the oxide layer SiO2, silicon nitride Si3N4,then photoresist and masking the layers followed by photolithography and etch of the silicon nitride and remove photoresist. then this is placed in oxidation furnace, helps in growing oxide layer in other regions. This process is called LOCOS "Local oxidation of silicon" and lastly we strip out Si3N4 using hot phosphoric acid.
- N-well and P-well formation: Both wells are created separately. First Pwell is created in steps like Photoresist, mask, photolithpgraphy and a p-type Boron material is diffused into psubstrate using Ion Implantation, forming P-well and same goes for n-well formation by diffusing a n-type Phosphorous material. Till now wells are created, by using High Temperature furnace process, drive-in dussion happens and  creating depths of wells and this process of creating is called tub process.
- Formation of gate - Gate is the most important terminal of CMOS transistor, where you control the threshold voltages and these voltages turns off/on the transistor.polysilicon layer is depositied and photolithography techniques are perfomed created NMOS and PMOS gates. The two parameters for gate formation are
   - Oxide capacitance
   - doping concentration    
- lightly doped drain(LDD) formation - To prevent hot electron effect and short channel effect
- Source & drain formation: In this, a thin layer of oxide is added to avoid the effect of channel during implants. It is where the vector velocity of ions matches with that of the crystalline structure of the p substrate and go directly into the substrate without hitting any silicon atom, when we do alot of ion implanatation. N+,P+ implants are done by Aresenic implantation and Hight temperature annealing.
- Local interconnect formation: Remove the thin screen oxide by etching in (hydrofloric) HF solution. There are various step to form local interconnects. One is
   - deposit titanium on wafer using supttering - Once it is depoisted and heating is done in N2 ambient, chemical reaction happens creating two kind of metal contacts, titanium silicon dioxide which are low resistant,used for interconnect contacts and titanium nitride(etched using RCA cleaning) used for the connection you want bring to the top,local communication
- Higher level metal formation: Non Planar surface topography is not suitable for metal interconnects. Planarize the surface by doping silicon oxide with Boron or Phosphorous to polish the surface. This technque is called chemical Mechanical polishing (CMP). Then we perform Photolithography techniques follwed by  TiN and balnket Tungsten layer deposition and do CMP. Deposit AL layer and repeat photolithography and CMP. This is 1st level of interconnect. Add another layer of interconnects to take to higher level metal layers. Lastly, Layer a dielectric i.e., Si3N4 to protect the chip. 

 ![Screenshot (34)](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/ae5675b9-5b61-4472-81c5-c0cb8f282529)

### SKY130 basic layer layout and LEF using inverter

From Layout, we see the layers which are required for CMOS inverter. Inverter is Pmos and Nmos connected together i.e., gates that is poly(red color), connected to input(A), NMOS source connected to ground, PMOS Drain connected to VDD, PMOS Source and NMOS Drain are connected together and fed to output(Y) 

The First layer in skywater130 is ``localinterconnect layer(locali)`` , above that metal 1 is purple color and metal 2 is pink color.

If you want to see connections between two different parts, place the cursor over that area and press S three times. The tkson window gives the connectivity information.

![image](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/fc51c2de-26f5-4ada-8c39-f5f29604999b)

### Library exchange format (.lef)
- The layout of a design is defined in a specific file called LEF.
-  It includes design rules (tech LEF) and abstract information about the cells. 
    - ```Tech LEF``` -  Technology LEF file contains information about the Metal layer, Via Definition and DRCs.
    - ```Macro LEF``` -  Contains physical information of the cell such as its Size, Pin, their direction.




