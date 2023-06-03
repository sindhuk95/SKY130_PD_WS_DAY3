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
   - Designing standard cell and SPICE extraction in MAGIC 
3. SKY130 Tech File Labs

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
somehow I figured it was due incorrect proxy, so I used this command  `` git config --global --unset https:proxy``. It worked and could clone the files from github to my openlane.

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

- From Layout, we see the layers which are required for CMOS inverter. Inverter is, PMOS and NMOS connected together.
- Gates of both PMOS and NMOS are connected together and fed to input(here ,A), NMOS source connected to ground(here, VGND), PMOS source is connected to VDD(here, VPWR), Drains of PMOS and NMOS are connected together and fed to output(here, Y). 

The First layer in skywater130 is ``localinterconnect layer(locali)`` , above that metal 1 is purple color and metal 2 is pink color.

If you want to see connections between two different parts, place the cursor over that area and press S three times. The tkson window gives the connectivity information.

![image](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/fc51c2de-26f5-4ada-8c39-f5f29604999b)

### Library exchange format (.lef)
- The layout of a design is defined in a specific file called LEF.
-  It includes design rules (tech LEF) and abstract information about the cells. 
    - ```Tech LEF``` -  Technology LEF file contains information about the Metal layer, Via Definition and DRCs.
    - ```Macro LEF``` -  Contains physical information of the cell such as its Size, Pin, their direction.
 
# Designing standard cell and SPICE extraction in MAGIC 

  -  First we need to provide bounding box width and height in tkson window. lets say that width of BBOX is 1.38u and height is 2.72u. The command to give these values to magic is
   ``` property Fixed BBOX (0 0 1.32 2.72)  ```
  - After this, Vdd, GND segments which are in metal 1 layer, their respective contacts and atlast logic gates layout is defined
Inorder to know the logical functioning of the inverter, we extract the spice and then we do simulation on the spice. To extract it on spice we open TKCON window, the steps are
   - Know the present directory - ``pwd ``
   - create an extration file -  the command is  `` extract all `` and  ``sky130_inv.ext`` files has been created
          
   - create spice file using .ext file to be used with our ngspice tool  - the commands are  
      ``` ext2spice cthresh 0 rthresh 0 ``` - extracts parasatic capcitances also since these atre actual layers - nothing is created in the folder
      ``` ext2spice ``` - a file ```sky130_inv.spice``` has been created.
      
     ![image](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/80394fcd-2b43-49fb-b51b-af80304888a0)
     
 ### SKY130 Tech File Labs
 
 # Creating SPICE Deck using Sky130 tech
      
  let us see what is inside the spice file 
  
  ![image](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/ea44dae6-87a4-4904-835d-74a096213306)

   In the spice file subcircuit(subckt), pmos and nmos node connections are defined
   For NMOS  ``` XO Y A VGND VGND sky130_fd_pr_nfet_01v8 ``` . The order is  ``` Cell_name Drain Gate Source Substrate model_name ``` .
   For PMOS  ``` X1 Y A VPWR VPWR sky130_fd_pr_pfet_01v8 ``` . The order is   ``` cell_name Drain Gate Source Substrate model_name ```.
   
  For transient anaylsis, we would like to define these following connections and extra nodes for these in spice file
   - VGND to VSS
   - Supply voltage from VPWR to Ground - extra nodes here will be 0 and VDD with a value of 3.3v 
   - sweep in/pulse between A pin and VGND (0)
  Before, editing the file, make sure scaling is proper, we measure the value of the gride size from the magic layout and define using `` .option scale=0.01u`` in the Deck file.
  
  ![size of row](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/ec4a0ff1-9009-4365-b66c-9e352ba1842a)
  
  Now keeping the connection in mind, define the required commands in the file. Along with this we need to include libs for nmos ``nshort.lib`` and pmos ``pshort.lib`` and define transient analysis commands too. We comment the subckt since we are trying to input the controls and transient analysis also. Model names are changed to ``nshort_model.0`` and ``pshort_model.0`` according to the libs of nmos and pmos.
  
  These voltage sources and simulation commands are defined in the Deck file.
   ``
   VDD VPWR 0 3.3V
   VSS VGND 0 0V
   Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)
   .tran 1n 20n
   .control
   run
   .endc
   .end
   ``
  
    
 ![image](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/f55eb2a7-0251-4fbd-9b40-24a8088f3bbb)
  
### Using ngspice for spice simulation
  
 Spice Deck is done and now to run spice simulation invoke ngspice in the tool and pass the source file. 
 
   ``` ngspice sky130_inv.spice ```
   
   On the prompt you can see the values the ngspice has taken. To see the plot, use
   
   ``` plot y vs time a ``` .
   
   Now we have the transient response, the next objective is to characterise the cell.
   
    ![response and plot](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/98e39f48-6674-49cd-8d67-646010419827)

     
 ### Standard cell characterization of CMOS Iinverter 
 characterization of the inverter standard cell depends on Four timing parameters

 Rise Transition: Time taken for the output to rise from 20% to 80% of max value
 Fall Transition- Time taken for the output to fall from 80% to 20% of max value
 Cell Rise delay = difference in time(50% output rise) to time(50% input fall)
 Cell Fall delay = difference in time(50% output fall) to time(50% input rise)
 
 The above timing parameters can be computed by noting down various values from the ngspice waveform.
 
 ``` Rise Transition : 2.25421 - 2.18636 = 0.006785 ns / 67.85ps ```
 
 ![rise transition](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/207d7bbb-cb8a-46d2-b921-a915eeac492a)
 
 ``` Fall Transitio : 4.09605 - 4.05554 = 0.04051ns/40.51ps ```
 
 ![falll trans](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/9b640ee5-ba9e-4552-8363-a38380c01ca9)
 
 ```Cell Rise Delay : 2.21701 - 2.14989 = 0.06689ns/66.89ps ```
 
![50% of f-r](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/4a969853-e7a1-48ae-b240-852e6bf4af55)

``` Cell Fall Delay : 4.07816 - 4.05011 = 0.02805ns/28.05ps ```

![50% off r-f](https://github.com/sindhuk95/SKY130_PD_WS_DAY3/assets/135046169/ad5fde3c-3c22-4076-8d12-e83002782b76)

### LAB exercise : MAGIC DRC




   
    
  
  
  
  

  

   
  
    



