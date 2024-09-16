# vsd_2024
Arduino Board:- This is an arduino microcontroller board. The encircled area shows the chip(microprocessor) which is interfaced with other components of the board. The designing of this chip from abstract level all the way down to the fabrication is done by RTL to GDSll flow.Arduino consists of both a physical programmable circuit board (often referred to as a microcontroller) and a piece of software, or IDE (Integrated Development Environment) that runs on the computer, used to write and upload computer code to the physical board.

![image](https://github.com/user-attachments/assets/dab1bdfe-27ef-4919-9258-dc0d092a8054)


chip components
(1) Pads: Through which we can send the signal inside the chip.

(2) Core: Place where all the logic gates are fixed.

(3) Die: Present at the corner. it is the size of the entire chip.

EX. RISC-V SoC:- It consist of SRAM,SOC,ADC,DAC,SPI these all are called foundary IP's.All devices depends upon foundary where all chips are fabricated using deposition and lithography techniques and so on.



Introduction to RISC-V
RISC-V, where five refers to the number of generations of RISC architecture that were developed at the University of California, Berkeley. RISC is an open standard instruction set architecture (ISA) based on established RISC principles. Unlike most other ISA designs, RISC-V is provided under open source licenses that do not require fees to use. A number of companies are offering or have announced RISC-V hardware, open source operating systems with RISC-V support are available, and the instruction set is supported in several popular software toolchains.
![image](https://github.com/user-attachments/assets/c560558b-c148-41f4-9951-7d38e4ccf04e)


The instruction set is designed for a wide range of uses. The base instruction set has a fixed length of 32-bit naturally aligned instructions, and the ISA supports variable length extensions where each instruction can be any number of 16-bit parcels in length. The instruction set specification defines 32-bit and 64-bit address space variants. The specification includes a description of a 128-bit flat address space variant, as an extrapolation of 32 and 64 bit variants, but the 128-bit ISA remains "not frozen" intentionally, because there is yet so little practical experience with such large memory systems. Chip is connected to the package with the help of bond wires.


![image](https://github.com/user-attachments/assets/fe045f2f-cd3a-4720-8cde-33c82296431f)

From Software Applications to Hardware
Here we will se how apps runs on the system?

Application Software - System Software - Hardware chip

Apps enters into a block of system software and system sodtware converts the entire program into binary language. There are some layers inside the system software whish are as follows

Operating System, Compiler, Assembler

Operating system handles input/output operations and allocate memory also it manage the low level system functions.

Compiler takes the output from the operating system as C,C++,Java and convert them into intsructions. These instructions depends upon hardware.

Assembler take the instructions from compiler and convert them into respective binary numbers. This binary language now send to hardware and hardware performs ouput based on the function it recieve and gives the output.

Instruction acts as abstract interface between C-language and the hardware.



Soc design and OpenLANE
Introduction to all components of open-source digital asic design
To design Digital ASIC, few tools or things which are required from the day one. These are

RTL Design EDA tools PDK data what is RTL design? In digital circuit design, register-transfer level (RTL) is a design abstraction which models a synchronous digital circuit in terms of the flow of digital signals (data) between hardware registers, and the logical operations performed on those signals.for this designs many open sorces are available. like, librecores.org, opencores.org, github.com, etc...

What is EDA tools? The term Electronic Design Automation (EDA) refers to the tools that are used to design and verify integrated circuits (ICs), printed circuit boards (PCBs), and electronic systems, in general. many open sorces tools are available like Qflow, OpenROAD, OpenLANE, etc...

What is PDK Data? PDK is process design kit. It is interface between FAB and design. This data is collections of files like,

process design rules: DRC, LVS, REX Digital standerd cell libreries i/o librerirs etc..... which are used to model a fabrication process for the EDA tools used to design an ICs. for example, in 2020, google release the open source PDK for FOSS 130nm production with the skywater technology. But right now it is at cutting age of the 5 nm also. But in many applications, the advance node is not required, and the cost of advanced node is also high as compared to 130nm processors. This 130nm processors are also fast processor. for example,

intel: P4EE @3.46 GHz(Q4'o4)

sky130_OSU (single cycle RV32i CPU) pipeline version can achieve more than 1 GHz clock.

Simplified RTL2GDS flow
![image](https://github.com/user-attachments/assets/38534bcd-fdf2-4d19-a885-7ff980f42cb8)


Step 1. Synthesis:- In the synthesis, the design RTL is translated to a circuit out from the SCL. The resultant circuit is describes in HDL and usualy refered to the gate level netlist. the gate level netlist is functionaly equivelent to the RTL. "standard Cells" have regular layouts like Electrical. HDL,SPICE

Step 2. Floor/Power Planning:-The main objective here is that to plan silicon area and distribute the power to the whole circuit. In the chip floor planning, the partition chip die between different system building blocks and place the i/o pads. In micro floor planning, we define the dimensions, pin locations, rows. In power planning, the power network is connstructed. tipically, the chip is power by multiple VDD and GND. so, total components are connected to power supply horizontaly and vertically by metal streps. here parallel structures are used to reduce the resistance. To address the electromagnetization problem, power distribution network uses upper metal leyers, which are thicker than lower metal layers. Hence have less resistance.



Step 3. Placement:- In this process, we place the gate level netlist on the floor planning rows, alligned with the sites. cells should be placed very closed to eachother to reduce the interconnnect delay. Usually placement is done in 2 steps:

Global placement:- It is very first stage of the placement where cells are placed inside the core area for the first time looking at the timing and congestion. Global Placement aims at generating a rough placement solution that may violate some placement constraints while maintaining a global view of the whole Netlist.

Detailed placement:- In detailed placements, we determined the exact route and layers for each netlist. the objective of detailed placement is valid routing, minimize area and meet timing constrains. Additional objective is minimum via and less power.



Step 4. Clock Tree Synthesis:- Before routing the signals, we have to route the clock. In the process of clock synthesis, we have distribute the clock to the every sequential elements. for example flipflops, registers, ADC, DAC ete. basically clock netwroks looks likes a tree. where the clock source is roots and the clock elements are end leaves. Synthesization should be done in a manner that with minimum skew and in a good shape.To minimize the clock skew by using the low-skew global routing resources for clock signals.Microsemi devices provide various types of global routing resources that significantly reduce skew.Usually a tree is a H tree, X tree etc.



Step 5. Routing:- After routing the clock, the signal routing comes. Making physical connections between signal pins using metal layers are called Routing. Routing is the stage after CTS and optimization where exact paths for the interconnection of standard cells and macros and I/O pins are determined. There are two types of nets in VLSI systems that need special attention in routing:

Clock nets Power/Ground nets The sky130 PDK defines the 6 routing leyers. the lowest leyer is called local interconnect layer (titanium nitride layer). Other five layers are alluminium layersIn the proccess of routing, metal trackes forms a routing grids and these grids are huge. so, devide and conquer approach is use for routing. The two types of routing is used:

Global routing: Generates the routing guides

Detailed Routing: Uses the routing guides to implement the actual wiring.


Step 6. Sign Off:- Once the routing is done, we can construct the final layout. This final layout will goes under the verification. Two types of verifications are there:

Physical verification: Here design rule checking will done and it will check the final layout and owners layout Timing Verification: Here Static Timing Analysis will done.

Introduction to OpenLANE and Strive chipsets
OPENLANE is an automated RTL to GDSII flow that is composed of several tools such as OpenROAD, Yosys, Magic, Netgen, Fault, CVC SPEF-Extractor, CU-GR, Klayout and a number of scripts used for design exploration and optimization. It is started as an Open-source flow for a true Open Source tape-out Experiment. striVe is a family of open everything SoCs: Open PDK, Open EDA, Open RTL

striVe SoC Family



The main goal of OPENLANE is to produce a clean GDSII with no human intervation (no-human-in-the-loop). here the meaning of clean is that:

No LVS violations

No DRC Violations

No timing Violations

OPENLANE is tuned for skyWter130nm open PDK. it can be used to harden Macros and chips.there is two mode of operation Autonomus : it is the push botton flow. with the push botton , it is a some time base design and due to this push botton, we get final GDSII

interactive : here we can run comamds and steps one by one.

It has large number of design examples(43 designs with their best configurations).

Introduction to OpenLANE detailed ASIC design flow


The design exploration utility is also used for regression testing(CI). we run OpenLANE on ~ 70 designs and compare the results to the best known ones.

DFT(Design for Test) it perform scan inserption, automatic test pattern generation, Test patterns compaction, Fault coverage, Fault simulation.After that physical implementation is done by OpenROAD app. physical implementation involves the several steps:

Floor/Power Planning

End Decoupling Capacitors and Tap cells insertion

Placements: Global and Detailed

Post Placement Optimization

Clock Tree synthesis (CTS)

Routing: Global and Detailed

Every time the netlist is modified.(CTS modifies the netlist and Post Placements optimization also modifies the netlist).so for that verification must be performed. The LCE(yosys) is used to formally confirm that the function did not change after modifying the netlist. ### Dealing with antenna rules Violation: when a metal wire segment is fabricated, it can act as antenna.as an antenna, it collect charges which can demaged the transister gates during the fabrication.



To address this issue, we have to limit the lenght of the wire. usually this is the job of the router. If router fails to do this, then there are two solutions: Bridging attaches a higher layer intermediary.Add antenna diode cell to leak away charges.(Antenna diodes are provided by the SCL)



With OpenLANE, we took a preventive approach. here we add fake antenna diode next to every cell input after placement. Then run the Antenna checker on the routed layout. If the checker reports a violation on cell input pin, replace the fake diode cell by a real one.



Static Timing analysis(STA) It involves the interconnect RC Extraction(DEF2SPEF) from the routed layout, followed by STA on OpenSTA(OpenROAD) tool. resulting report will shows the timing violations if any violations is there.

Physical Verification (DRC and LVS) Magic is used for design Rules checking and SPICE Extraction from Layout. Magic and Netgen are used for LVS.

Get familiar to open-source EDA tools
OpenLANE Directory structure in detail
Basic Linux Commands

cd : opens the particular folder

ls : lists the content of the folder

pwd : shows the present working directory

mkdir : to make a new directory

command --help : shows the complete use that command

clear : clears the terminal screen

Here we are working in Sky130_fd_sc_hd PDK varient. where, "sky130" is process name or node name."fd" is a foundary name (skyWater foundary)."sc" means standerd cell librery files and the last one "hd" stands for high density(basically one type of varient).

Sky130_fd_sc_hd varient contains many technology files like verilog, spice, techlef, meglef,mag,gds,cdl,lib,lef,etc. (techlef file contains the layer information).

Design Preparation Step
when we enter in the OpenLANE, we have to use flow.tcl because as a name says, it will goes with the flow using the script. And by using interactive switch, we will do step by step process. without interactive switch, it will run complete flow from RTL to GDSII. Now OpenLANE is open and we can see that prompt will change now.

![image](https://github.com/user-attachments/assets/160c0c45-9e9b-4c14-af80-e6cf138a07d5)


Now we have to input all the packages which required to run the flow. Here we are ready to execute the command.
![image](https://github.com/user-attachments/assets/3a834ab8-add3-42aa-8586-53645152f104)

Now, if we are going into the design folder in openlane, there are nearly 30-40 designs are already builted. Out of them we can open any of the design. for example, here we are opening the picorv32a.v design. In this design we can see many files are available. i.e., scr, config.tcl, etc. This config.tlc file contains every details about the design. for example, details about enrollment, clock period, clock period port etc.

![image](https://github.com/user-attachments/assets/75223df2-8e65-4ace-87c1-3948200117e4)


Here we can see that the time period is set to the 5.00 nsec. but is we see in the openlane sky130_fd_sc_hd folder, the period is set about 24 nsec. so it is not override to the main file. If it override then give first priority to the main folder.

Now, in openlane, we are going to run the synthesis, but before synthesis, we have to prepare design setup stage. for that command is  prep -design picorv32a
![image](https://github.com/user-attachments/assets/502e04d2-ae2b-4190-a607-a3b9cc9e9fdd)

So, here it is shown that preparation is completed.

Review files after design prep and run synthesis
After completing the preparation, in the picorv32a file, the run terictory is created. Inside the folder, Today's date is created. so in this terictory some folders are available which is required for openlane.

In the temp file, merged.lef file is available which was created in preparation time. if we open this merged.lef file, we get all the wire or layer level and cell level information.
![image](https://github.com/user-attachments/assets/1230835d-2cf0-476b-97d3-5a102549b4eb)



While, in the result folder is empty because till we have not run anything and in the report folder all the folders are there about synthesis, placement, floorplanning,cts,routing,magic,lvs.

now here also one config.tcl file is available similar like design folder. But this config.tcl file contains all default parameter taken by the run.


when we make some change in the origional configuration and then we run, for example if we make a change in core utilization in the floorplanning and then we run the floorplanning, at this time in the congig.tcl file, the core utility will change and by cross checking it we can check that the modification is reflected in the exicution or not.

Now coming to the openlane, we are going to run the synthesis. for that command is run_synthesis It will take some 3-4 mnts to run the synthesis and finally synthesis will complited.
![image](https://github.com/user-attachments/assets/0472ef05-982b-436c-9238-1b5e9cfde85c)
![image](https://github.com/user-attachments/assets/25e8fa8a-c5ac-4b6a-bef8-fc5975468245)


Steps to characterize synthesis results
From the data of synthesis, total number of counter D_flip-flops is 1613. and the number of cells is 14876.
![image](https://github.com/user-attachments/assets/f17d45d1-294e-4371-8ace-f92730a1b27c)



So, the flop ratio = (number of flip flops)/(number of total cell).

So, the flop ratio is 10.84%.

Before run, we saw that the result folder is empty. but now, after running the synthesis, we can see that all the mapping have been done by ABC.
![image](https://github.com/user-attachments/assets/34bc1cc1-5cf3-49d6-927d-83c11b13cb3b)



And in the report, we can see when the actual synthesis has done. and the actual statistics synthesis report is showing below, which is same as what we have seen before.

![image](https://github.com/user-attachments/assets/d8e979b6-ab22-4047-8f4d-cc499d79deca)


Day 2 - Good floor planning considerations
Chip Floor planning consideration
Utilization factor and aspect ratio
In this section we will try to cover up the width and height of Core and Die. It is the first step in physical design flow to find out the width and height. Let's begin with a netlist, netlist is two flipflops and have a simple combination logic in between. A netlist describes the connectivity of an electronic design. Here, we dependent on the dimensions of the logic gates(AND & OR) and particular flipflop. Now, let's convert the symbols into physical dimensions. We are interested in the dimensions of the Core and Die not in the dimensions of the wires.

Let's standard cell have dimensions of 1unit*1unit

So, area= 1 Sq. units

Asuume same area for the flipflop as well = 1 Sq. units

with help of these dimensions and netlist let's calculate the area occupied by the netlist on a silicon wafer.



Befor that will remove all the wires and bring all the flip flops and logic gates in a single plate. So after combining them together width and length will be 2 Sq. units each and if we calculate the total area the it will be 4 Sq. units. So now we have the rough calculation of minimum area occupied by the netlist.



What is 'Core' and 'Die' section of a chip?

Let's have a silicon wafer on which all the logics are implemented. In thes one section is refered as 'Die' and inside the Die we have the Core.

A Die which consists of core, is small semicondcutor material specimen on which the fundamental circuit is fabricated.

A 'Core is the section of the chip where the fundamental logic of the design is placed.



Now, Let's try to place that particular logic inside the core. The netlist will occupy the whole area inside the core it means it utilizes the core 100%. From this we can calculate the utilisation factor which is given by,

        Utilization Factor = Area occupied by netlist / Total area of the core
   
 lets put the dimensions we have, we get
 
 Utilization factor = 4*1sq.unit / 2unit *2unit
 
		= 4sq unit /  4sq unit
So, utilization factor = 1 (It means core has utilized all the area and no spane left)

Aspect Ratio = Height / width = 2 unit / 2unit = 1

Whenever Aspect Ratio is 1 it signifies that chip is square shaped. When it is not 1 it means the chip is in rectangular shape.



For example, Lets take another dimensions of the width= 4unit and height = 2unit. So from the above formula of utilization factor we it equal to 0.5 which means the chip has not covered the whole area of the core and aspect ratio is also 0.5 which means the chip is rectangular in shape.

The leftover area can be used to placed some additional cells like buffers or something else.



Utilization factor and aspect ratio
Lets take another example for a square chip wth dimensions 4*4 sq units. We will get utilization factor= 0.25 it means out of the whole chip area only 25% area is utilized by the netlistand 75% is available for additional cells which can be use for routing in which we will have layering. Aspect ratio we get = 1 it means chip is square in shape.



Define locations of Preplaced Cells:- Lets take a combinational logic which does some amount of function and assume its a huge circuit having some N Logic gates so let's devide it into some small numbers of gates. We will cut the whole circuit into two parts, and separate both of them into two blocks and both block will be implemented seperately.



In both the blocks lets extend the input output pins and now we will black box the boxes and detached them. After black boxing, the upper portion is invisible from the top or invisible to the one , who is looking into the main netlist. now will seperate them out as two different IP's or modules.



Advantage of doing this is we can reuse them multiple times after implimenting once only. Similary there are other IP's also available for eg. Memory, Clock-gating cell, Comporator, MUX all of these are part of the top level netlist.They recieve some signals and perform functions and deliver the outputs but the functionality of the cell is implemented only once.

The arrangement of these IP's in a chip is refferd as floorplanning.

These IP's have user-defined locations, and hence are placed in chip before automated placement and routing are called "pre-placed cells".

These cells are placed in such a way that, the placement and routing tool do not touch the location of the cell.

De-coupling capacitors
surround pre-placed cells with Decoupling capacitor:- Let consider some circuit, which is the part of the blocks which has been described earlier. When some gate (let consider AND gate) switched from 0 to 1 or 1 to 0, considered amount of the switching current required because of available small capacitance . This capacitor should be completely charged to represent logic 1 and completly discharged to represent logic 0. Consider capacitance to be 0. Rdd,Ldd,and Lss are well defoned values. During switvhing operation, the circuit demands switching current i.e. peak current. Now, due to the presence of Rdd and Ldd, there will be a voltage drop across them and the voltage at Node 'A' would be Vdd' instead of Vdd.



So, due to this if ideal logic 1 = 1 volt then here practically it can be less then 1 volt i.e., 0.97 volts (Vdd'). So, for any signal to be considered as Logic '0' and '1' in the NM low and NM high range. It is danger case.



To solve this problem,, we have to put De-coupling capacitor in parallel with the circuit. Every time the circuit switches, it draws current from Cd, whereas, the RL network is used to replenish the charge into Cd. And the amount of current needed for the circuit is supplied by the De- Coupling Capacitor.


In the chip it will look something like shown below Decoupling capacitors are placed in between the block a, block b and block c. So here in this whole block it has been ensured that supply is being done by the de-coupling capacitor. Once we are done with this we have taken care of the local communication.



Power planning
Now let's consider that local circuitory and keep it as a black box and it can be repeat multiple times and there is some logic present at the boundaries also and the problem of current demand was solved by de-coupling capacitor. There is signal which is send from driver to load and the signal is basically logic 0 to logic 1. Here we need to maintain the particular driver to load line with same signal so that the load recieves the same. Now power supply is applied. Now assume 16 bit bus has to retain the same signal from driver to the load. so it should get the sufficient power from the supply. But at this bus, there is no de-coupling capacitor is available because it is not physible to put capacitor at all over the place. now, power supply is far away from the bus, that is why some voltage drop between them will occur definetly.



When we say one particular line of 16-bit bus is logic 1 it says that the capacitor is being charged to Vdd, and whenever we say logic 0 it says that the capacitor is discharged to ground.Let consider this 16 bit bus connected to inverter. So, all the capacitor are initially charged will get discharged and vice-versa due to inverter.



But the problem is occurs due to all capacitor is connected to the single ground. This will cause a bump in 'ground' tap point during discharging. That bump is called as Ground Bounce. If the size of the bump exceeds the noise margin levelit might enter into an undefined state and due to undefined state it can either go to logic 1 or logic 0. So here thing becomes unpredictable



Also , all capacitors which were'0' volts will have to charge to 'V'volts through single 'vdd'tap point. This will cause lowering of voltage at Vdd tap point. As long as this voltage drop is in noise margin level we are good enough but if it goes into an undefined region then things become unpredictable.



The phenomenon we have seen was causing the lowering of the supply voltage,this problem occured because power has applied to one point only. The solution of the problem is use multiple power supply. So, every block will take charge from neartest power supply and similarly dump the charge to the nearer ground. this type of power supply is called mesh.



And the power planning is shown below,

![image](https://github.com/user-attachments/assets/12fb9f21-80cb-4007-afbc-332b632ef49a)


Pin placement and logical cell placement blockage
Pin Placement

Lets take below designs for example that needs to be implemented. Here first circuit is driven by clk1 and second circuit is driven by clk2 and both has different inputs Din1 and Din2 respectively and outputs as Dout1 and Dout2.Along with that we have some preplaced cells as well as Blocka which recieves inputs from Din1 second input from Din2. We have another preplced cell as Blockb Which recieves input from clk1 and clk2 and provides a clk output. So currently we have 4 input ports Din1,Din2,Clk1,Clk2 and 3 output ports Dout1,ClkOut,Dout2



let's have one more design that needs to be implemented. this types of circuits are very much helpful to understand the timing analysis of inter clocks. now complete design becomes like given below which has 6 input ports and 5 output ports. The connectivity information between the gates is coded using VHDL/Verilog language and is called as 'Netlist'.


Let's put this netlist in the core which we have designed before and let's try to fill this empty area between core and die with the pin information. The frontend team who decides the netlist connectivity input and output and the backend team who done the pin placements. So according to the pin placements, we have to locate the preplaced blocks nearer to the inputs of the preplaced blocks.



Here one thing that we noticed is that clock-in and clock-out pins are bigger in size as compared to input and output pins. reason behind this is that, input clocks are conntinuously provides the signal to the every elements of the chip and output clock should out the signal as fast as possible. So, we need least resistance path for the clocks inputs and clocks outputs. So, bigger the size, lower the resistance.

One more thing is need to take care about is that, this pin placement area is blocked for routing and cell placements. so we nned to do logical cell placement blockage. this blockage is shoown in above image in between pins.

So, floor plan is ready for Placement and Routing step.

Steps to run floorplan using OpenLANE
Before run the floorplanning, we required some switches for the floorplanning. these we can get from the configuration from openlane.



Here we can see that the core utilization ratio is 50% (bydefault) and aspect ratio is 1 (bydefault). similarly other information is also given. But it is not neccessory to take these values. we need to change these value as per the given requirments also.

Here FP_PDN files are set the power distribution network. These switches are set in the floorplane stage bydefault in OpenLANE.



Here, (FP_IO MODE) 1, 0 means pin positioning is random but it is on equal distance.

In the OpenLANE lower priority is given to system default (floorplanning.tcl), the next priority is given to config.tcl and then priority is given to PDK varient.tcl (sky130A_sky130_fd_sc_hd_congig.tcl).

Now we see, with this settings how floorplan run.

Review floorplan files and steps to view floorplan
In the run folder, we can see the connfig.tcl file. this file contains all the configuration that are taken by the flow. if we open the config.tcl file, then we can see that which are the parameters are accepted in the current flow.



To watch how floorplane looks, we have to go in the results. in the result, one def( design exchange formate) file is available. if we open this file, we can see all information about die area (0 0) (660685 671405), unit distance in micron (1000). it means 1 micron means 1000 databased units. so 660685 and 671405 are databased units. and if we devide this by 1000 then we can get the dimensions of chips in micrometer.



so, the width of chip is 660.685 micrometer and height of the chip is 671.405 micrometer.

To see the actual layout after the flow, we have to open the magic file by adding the command magic -T /home/kunalg123/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def

And then after pressing the enter, Magic file will open. here we can see the layout.



Review floorplan layout in Magic
In the layout we can see that, input output pins are at equal distance.



after selecting (To select object, first click on the object and then press 's' from keyboard. the object will hight lited. to zoom in the object, click on the object and then press 'z' and for zoom out press 'sft+z') one input pin, if we want to check the location or to know at on which layer it is available, we have to open tkcon window and type "what". it will shows all the details about that perticular pin.



so, it show that the pin is in the metal 3.similarly doing for the vertical pins, we find that this pin is at metal 2.



Along with the side rows,the Decap cells are arranged at the border of the side rows.


here we can see that first standerd cells is for buffer 1. similarly other cells are for buffer 2, AND gate etc.



Library building and Placement
Netlist binding and initial place design
Bind netlist with physical cells:- Lets we have the netlist of gates and shape of these gates represents the functionality of this gates. Foe example we have NOT gate as a tringular shape but in reality it is a box with physical dimensions it has width and height.Similarly for AND gate it also has a box shape in reality, Flipfops are also square boxes.So, we have given the physical dimensions to all the gates and flipflops. For everycomponent of the netlist we will give the particular shape with particular dimensions because ir real world the shapes like AND,OR gates does not exists so we make them as square all the blocks also have the width and height and proper shape.



Now we will remove the wires,all the gates, flipflops and blocks are present in the shelf which is called as Library.

A library is a place where you can find all kind of books all the gates,f/f are books here. Library also has the timing information of the perticular book like delay of the gates. Library can be devides into two sublibraries, One library consist of shape and size and other library might consist only of the delay information. Library has the various flavours of each and every cell. Like same cell can have bigger in size in different self, bigger the size of cell lesser the resestnce path so it will work faster and will have lesser delay. We can pick up from these what we want based on the timing condition and available space on the floorplan.



Placement:- Once we have given proper shape and size to each and every gates the next step is to take those particular shapes ans sizes and place it on the floorplan. We have the floorplan with inout and output ports, we have particular netlist, and we have particular size given to each component of this netlist. So we have the physical view of the logic gates. Next step is to place the netlist onto the floorplan. We have to take the connectivity information from the netlist and design the physical view gates on the floorplan.



Now, we have the floorplan where we have the preplaced cells from the previous slides, Plcement will make syre that the pre placed cells locations are not affected they are kept as it as and the second thing which will be taken care of that is no cell should be placed over the pre-placed cells. We need to place the physical view of the netlist onto the floorplan in such a fashion that logical connectivity should be maintained and that particular circuit should interact with their input and output ports to maintain the timing and the delay will be minimal.



Here first we will see the arrangement of the remaining parts from the netlist onto the floorplan.We have placed all the element in such manner that all elements are closed to it's input and output pins. But, the distance of FF1 of Stage 4 and Din4 is still far them others. By optimizing the placement, we can solve this problem.



Optimize placement using estimated wire-length and capacitance
Optimize Plecement:- In optimize placement we will resolve the problem of distancing.Lrt's take the example of FF1 to Din2. There must be a wire going from Din2 to FF1 but before going into routing the desing or wiring we will try to estimate the capacitances. If we lokk the capacitance from Din2 to FF1 it is every huge because wire length is huge in that case even the resutance will also be huge because of that length. If we send the signal from Din2 then it will be difficult for FF1 to catch that input because distance is large. So we can place some intermediate steps to maitain the Signal integrity. By this the input is succesfully driven to the FF1 from Din2. These intermediate steps are called here Repeaters , Repeaters are basically buffers that will recondition the original signal and make a bew signal which replicate the original signal and send it forward this process repeates untill we reach to the actual cell where we want to send the input in this way signal integrity is maintained. By using repeaters we resolve the problem of signal integrity but there will be a loose of area because more and more repeaters are used more area will be used of the particular floorplan.



In the stage 1, there is no need of any repeater to transmit the signal. But in stage 2, due to high distance, the lenth of wire is high and signal is not transmitted in perticular range. so we required repeater.



Final placement optimization
As similar to stage 2, in Stage 3 also we required the buffer between gate2 and FF2.



Stage 4 is bit tricky as compared to other stages.Now we have to check that, what we have done is correct or not. For that we need to do Timing analysis by considering the ideal clocks and according to the data of analysis, we will understand that, the placement is correct or not.



Need for libraries and characterization
Every ICdesign Flow needs to go through the several steps. First step to go through is Logic Synthesis, let's say if we have a functionality which is coded in a form of an RTL so first we need to convert the functionality into legal hardware is refered to as Logic Synthesis. Ouput of the logic synthesis is arrangement of gates that will represent the original functionality that has been described using an RTL. Next step of logic synthesis is Floorplaning, in this we omport the output of logic synthesis and decide the size of the Core and Die. The next step after floorplaning is Placement, in this we take the particular logic cell send place them on the chip in such a fashion that initial timing is better. Next step is CTS(Clock tree synthesis), in this we take care that clk should reach each and every signal at the same time also take care of each clk signal has equal rise and fall.Next step is Routing, routing has to go through the certain flow dependendent on the characterization of the flip flop.And now comes the last step STA(Static timing analysis), in this we try to see the set up time, hold time, maximum achieved frequency of the circuit. One common thing across all stages 'GATES or Cells'.

Congestion aware placement using RePlAce
Right now we are not constrain about timing, but constrain about the congestion. so, we are making the congrstion is less.

The placement is donne in two stages. Global and detailed. In global placement, legalization is not happened but after detailed placement legalization will be done.

When we run the placement, first Global placement is happens. main objective of glibal placement is to reducing the length of wires.

Now opening the Magic file to see actual view of standerd cells placement.And the actual view in the magic file is given below.



If we zooom into this, we find the buffers, gates, flip flops in this.



Cell design and characterization flows
Inputs for cell design flow
In Cell Design Flow, Gates, flipflops, buffers are named as 'Standard Cells'. These standard cells are being placed in the section called as 'Library'.And in the library many other cells are available which have same functionality but the size is different.


If you look into one of the inverter from the library the cell design flowis as follows

The inverter has to represented in form of the shape, drive strength, power charracteristic and so on. Here cell design flow is devided into three parts.

Inputs

Design steps

Outputs

1)Inputs:- Inputs required for cell design is PDKs, DRC and LVS rules SPICE models, library and user defined specs. In DRC& LVS rules tech file is provided which contains design rules and actual values. Rules can be converted in to code. SPICE MODEL tells about threshold voltage equation.



Circuit design steps
The seperation between the power rail and the ground rail defines the cell height. Cell width depends upon the timing and drive strength.

2)design steps:- Design involves three steps which are circuit design, layout design, characterization.

In circuit Design there are two steps.

First step is to implement the function itself and second step is to model the PMOS nad NMOS transistor in such a fashion in order to meet the libraray.

3)Outputs

The typical output what we get from the circuit design is CDL(circuit description language) file,GDSII,LEF,extracted spice netlist(.cir).



Layout design step
In Layout Design First step is to get the function implemented through the MOS transistor through a set of PMOS and NMOS transistor and the second step is to get the PMOS network graph and the nNMOS network graph out of the design that has been implemented.



After getting the network graphs next step is to obtain the Euler's path. Eule's path is basically the path which is traced only once.


Next step is to draw stick diagram based on the Euler's path. This stick diagram is derived out of the circuit diagram.



Next step is to convert this stick diagram into a typical Layout, into a proper layout and then get the proper rule we have discissed earlier. Once we get the particular layout then we have the cell width, cell length and all the specifications will be there like drain current, pin locations and so on.



Next and Final step is to extract the parasatics of that particular layout and charaterise it in terms od timing. So before that the output of the layout design will be GDSll. Once you get the extracted spice netlist then we characterize it. Characterization helps in getting timing, noiseand power information.

Typical characterization flow
Let's try to build the characterization flow based on the inputs we have,

First step is to read in the model, second step is to read the extracted spice netlist, third step is to define or recognize the behaviour of the buffer, fourth step is to read the subcircuits of the inverter and then in the fifth step need to attach the necessary power supplies, sixth step is to apply the stimulus then in the seventh step we need to provide the necessary output capacitance then in the final eighth step in which we need to provide necessary simulation command for example if we are doing transent simulation so we need to give .tran command , if we are doing DC simulation then we give .dc command.



Next step is to feed in all this inputs from 1 to 8 in a form of a configuration file to the characterization software "GUNA" .

This software will generate power, noise and timing model.



General timing characterization parameters
Timing threshold definitions
As seen in the previous section we have inverter connected back to back, we have power sources, we have the stimulus applied to the inverter all these things brings a very important point of understanding differenet threshold points of a waveform itself and it is called as "Timing threshold definitions'.

Propagation delay and transition time
Based on these above values we are going to calculate the further values like propogation delay, current,slews etc.

If we want to calculate the delay of anything we need to subtract the out_rise_thr from in_rise_thr. Here let's take typical value 50%, let's see on the particular waveform how does it works Time delay = Time(out_thr)-time(in_thr).

image

In the above example in_rise_thr and out_fall)thr was kept at 50%. But if the threshold ponit moves to the top the the output comes before the input and we see negative delay and negative delays are not accepted. So the reason behind having this negative delay is poor choice od threshold point so thr choice of the threshold point is really important.

image

Let's take another example where we have choosed threshold point correctly but still can get a negative delay. Because uotput comes before the input that's why we are getting negative delay here, which is not accepted

image

Transition time= time(slew_high_rise_thr)- time(slew_low_rise_thr)

or

transition time = time(slew_high_fall_thr)- time(slew_low_fall_thr)

Let's say we have the waveform to understand the slew calculation.

image

Day 3 - Design library cell using Magic Layout and ngspice characterization
Labs for CMOS inverter ngspice simulations
IO placer revision
Till now, we have done floor planning and run placement also. But if we want to change the floorplanning, for example, in our floor planning, pins are at equal distance and if we want to change it then we can also make it by Set command.

For that first we have to check the swithes in the configuration and from that we have to take the syntax "env(FP_IO_MODE) 1". and make it to the "env(FP_IO_MODE) 2". then again run the floorplanning.

Then check the changes in the pins location through magic -T.

image

So, here we can see that there are no pins in the upper half side. all pins are in the lower half of the core.

SPICE deck creation for CMOS inverter
VTC- SPICE simulations:-Here first part is to create SPICE deck, it's the connectivity information about the netlist so basically it's a netlist.It has input that are provided to the simulation and the deck points which will take the output.

Component connectivity:- In this we need to define the connectivity of the substrate pin. Substrate pin tunes the threshold voltage of the PMOS and NMOS.

image

Component values:- Values for the PMOS nad NMOS. We have taken the same size of both PMOS and NMOS.

image

Identify the nodes:- Node mean the points between which there is a component.These nodes are required to define the netlist.

image

Name the nodes:- Now we wiil name these nodes as Vin, Vss, Vdd, out.

image

Now we will start writing the SPICE deck. It's written like shown below

Drain- Gate- Source- Substrate

For M1 MOSFET drain is connected to out node, gate is connected to in node, PMOS transistor substrate and Source is connected to Vdd node.

For M2 MOSFET drain is connected to out node, gate is connected to in node, NMOS source and substrate are connected to 0.

image

SPICE simulation lab for CMOS inverter
Till now we have described the connectivity information about CMOS inverter now we will describe the other components connnectivity information like load capacitor, source. Let's seee the connectivity of output load capacitor.

It is connected between out and the node 0. And it's value is 10ff. Supply voltage(Vdd) which is connected between Vdd and node 0 and value of it is 2.5 , Similarly we have input voltage which is connected between Vin and node 0 and its value is 2.5.

image

Now we have to give the simulation commands in which we are swiping the Vin from 0 to 2.5 with the stepsize of 0.05. Because we want Vout while changing the Vin.

image

Final step is to model files. It has the complete description about NMOS and PMOS.

image

image

Now we will do the SPICE simulation for the particular values. And will get the graph.

image

Now, doing other simulation in which we change the PMOS width to 3 times of NMOS width. and after diong the simulation, we get the graph like this shown below

image

The difference between these two graphs is that in the second graph the transfer charactoristic is lies in the ecxact middle of the graph where in the first graph it is lies left from the middle of the graph.

Switching Threshold Vm
These both model of different width has their own application. By comparing this both waveform, we can see that the shape of the both waveform is same irrespective of the voltage level.It tells that CMOS is a very roboust device. when Vin is at low, output is at high and when Vin is at high, the output is at low. so the charactoristic is maintain at all kind of CMOS with different size of NMOS or PMOS. That is why CMOS logic is very widely used in the design of the gates.

Switching thresold, Vm (the point at which the device switches the level) is the one of the parameter that defined the robustness of the Inverter. Switching thresold is a point at which Vin=Vout.

image

In this figure, we can see that at Vm~0.9v, Vin=Vout. This point is very critical point for the CMOS because at this point there is chance that both PMOS and NMOS are turned on. If both are turned on then there are high chances of leakage current(Means current flow direcly from power to ground).

By comparing this both the graph we can understang the concept of switching thresold voltage.

image

In the graph below we can identify that the PMOS and NMOS are in which region. The direction of current flowing is different for NMOS nad PMOS.

image

Static and dynamic simulation of CMOS inverter
In Dynamic simulation we will know about the rise and fall delay of CMOS inverter and how does it varying with Vm. In this simulation everything else will remian same except the input which is provided will be a pulse and simulation command will be .tran

The graph Time vs Voltage will be plotted here from where we can calculate the rise and fall delay.

image

image

Lab steps to git clone vsdstdcelldesign
To get the clone, copy the clone address from reporetery and paste in openlane terminal after the command git clone. this will create the folder called "vsdstdcelldesign" in openlane directory.

image

now, if we open the openlane directory, we find the vsdstdcelldesing folder in the openlane directory.

image

Now if we goe to the vsdstdcelldesign folder and open it, we get the .mag file,libs file etc.

image

now, let's open the .mag file and see that which layers are used to build the inverter. But before opening the mag file, we need tech file. so we will copy this file from this given below address, And do copy by cp command to the location which is given below.Now, we can see that this file is copied in the vsdstdcelldesign folder.

image

Now, here to see the layout in magic, we don't need to write the whole address because we copy the tech file here.Now, we can see the layout of CMOS inverter in the magic like this.

image

Inception of layout ̂A CMOS faabrication process
Create Active regions
1) selecting a substrate:- we have a p-type silicon substrate having high resistivity(5-50ohm) well dopped, and orintation(100).

2) creating active region for transistor:- Region where you see PMOS and NMOS. On p-type substrate we are going to create some small pockets which will be called as active region and in these pockets we are going to create PMOS and NMOS transistor. Will cretae isolation between each and every pockets.

We create the isolation layer by depositing the Sio2 layer (~40nm) on the substrate. Now, we are depositing the Si3N4 layer (~80 nm) on the Sio2 layer.

image

Before creating the pocket identify the region where we need to crete the pocket. Now will deposite a layer of photoresist(~1um) on which we will create some mask1 using UV light.

image

Unwanted area has been exposed using UV light. And we get pattern the exposed area is getting washed away.

image

In the next step mask will be removed and doing etching of Si3N4 layer on the exposed area.

image

Now, next step is to remove photoresist by chamical reaction, because now to Si3N4 layer itslef behaves like good protecting layer for Sio2 layer. now,We will place it in the oxidation furnace. if we do LOCOS (local oxidation of silicon) process, the exposed sio2 part will grow and bird break also form. This grown sio2 will provide the perfect isolation between two PMOS and NMOS. This is how we protect two transistor communicating with each other.

image

Next step is to remove the Si3N4 using hot phospheric acid.

image

Formation of N-well and P-well
3) N-well and P-well formation:- we can not form P-well and N-well at a same time. we have to protect a region while forming one of the region by photoresist. And then using mask 2 and UV light, we will do patterning of photoresist to form P-well.

image

Now, the area where we want to form the P-well is exposed. now we remove the mask and by applying the ion implantaton method (~200kev)to form P-well using Boron. But still it is P implant. After performing the high temparature anneling, it will become P-well.

image

We wiil do a similar process to form N-well by using mask 3 and using Phosphorus ions.

image

Till now depth of wells are not define. so, by putting into the high temparature furnace (drive-in diffusion), we will define the depth of wells.

image

Formation of gate terminal
4)Gate formation:- Gate terminal is the most important terminal of the PMOS and NMOS because from the gate terminal only we can control the thresold voltage. doping concentration and oxide capacitance will control the thresold voltage.so, first we are maintain the doping concentration here. for that we use mask 4 and again doing the ion implantation of boron ion at lower energy (~60kev).

image

same process we will repeat for N-well also by using mask 5 and Arsenic ion.

image

Next step is that we have to fix the oxide layer. but before that we have to remove the oxide layer because this layer is got dammeged because of the privious processes. so,first we remove the layer using HF solution and again re-grown the high quality oxide layer with same thickness.

The final step is the deposition of polysilicon layer over oxide layer with more impurities for low resistance gate terminal.Then etched out this polysilicon layer by using mask 6 and photoresist.

image

After etching, remove the photoresist and gate terminal looks like,

image

Lightly doped drain (LDD) formation
5) LDD formation:- Here, we actully want P+,P-,N doping profile in the PMOS and N+,N-,P doping profile for NMOS. Reason for that is

Hot electron effect

short channel effect

For the formation of LDD, we again do ion implantation in P-well by using mask 7 and here we use phosphoros as a ion for light doping.

image

Same process we will repeat for N-well. there we use mask 8 and BOron Ion.

image

Now, by creating the spacers, we can protect the actual structre remain constant of P-implantt and N-implant. For that we deposite a thick Sio2 or Si3N4 layer over the gate tereminal.

image

Now, we do Plasma anisotropic etching. By that side-wall spacers are formed.

image

Source Ã�Â� drain formation
6)source-drain formation

Next step is deposite the very thin screen oxide layer to avoid the effect of channeling.

image

Now to form the drain and source, again we do the ion implantation of arsenic at 75kev to create the N+ implant by using mask 9 in the P-well to form PMOS.

image

Same process we will repeat for NMOS by using the mask 10 and boron ion in the N-well at 50kev to creat P- implant.

image

Now we put this Half made CMOS into the high temparature (1000 degree)anneling. So P+ implant and N+ implant now become the source and drain.

image

Local interconnect formation
7)steps tp form contacts and local interconnects:- First step is remove the thin screen oxide layer by etching. Then deposite the titanium (Ti) using sputtering. here Ti is used because Ti has very low resistivity.

image

Next step is to create the reaction between Ti layer and source, gate, drain of CMOS. For that wafer is heated at about 650-700 degree temparature in N2 ambient for about 60 seconds. and after reaction, we can see the titanium siliside over the wafer. One more reaction is heppend there between Ti and N. and it results the TIN which is used for local communication.

image

Now by using mask 11 and photoresist, we will etched out the TIN and make perticular contacts. TIN is etched out by using RCA cleaning.

image

Now, local interconnects are formed after etching and removing the photoresist.

image

Higher level metal formation
8)Higher level metal formation:- These steps are very semilar like previous steps. First thing that we are noticing is that the surface is non planner. it is not good to use this type of non planner serface for matel interconnects because of the problems regarding the metal disconinuty. so, we have to plannerize the surface by depositing the thick layer of sio2 with some impurity to make less resistive layer. and then we used CMP (chemical mechanical polishing) technique to plannerise the surface.

image

Now using mask 12 and photorsist we etched the sio2 layer to diposite the metal in it.

image

Now remove the photoresist and seposite the thin later of TIN (~10nm) over the wafer. Because TiN is act as very good adession layer for sio2 and also act as a barrier between bottom layer and top layer of metal interconnects.

image

Next step is to deposite the blanket tungsten (W) layer over the wafer. and then do the CMP here to plannerize the surface.

image

This W is act as a contact holes and this holes needs to connect to the Higher metal layer. so we will deposite the Al (aluminium) layer.

image

Then by using the mask 13 and photoresist, we etched the W layer out to form the contact at perticular place by Plasma etching.

image

This is our first level of metal interconnets. now we again do the same process as above to deposite the second level of metal interconnect by using mask 14 for etched out the sio2 and using mask 15 for etched out Al leyer.

image

The upper layer of Al is bit thicker as compared to lower layer of Al.Now, again deposite the layer of sio2 or si3N4 to protect the chip.

image

And finally our CMOS is looks like this after the fabrication.

image

Lab introduction to Sky130 basic layers layout and LEF using inverter
image

In sky130, every color is showing the different layer. here the first layer is for local interconnect shown by blue_purple color, then second layer is metal 1 which is shown by light purple color, and the metal 2 is shown by pink color. N-well is shown by solide das line. green is N-diffusion region. and red is for polysilicon gate. similarly the brown color is for P-diffusion.

In tckon window, we can see that the selected area is NMOS and similarly we can chech PMOS also. and that is how we can check that the CMOS is working or not.

image

similarly we will check for the output terminal also.(by double pressing "S" to select the entire thing at output Y).

image

so, we can see that "Y" is attached to locali in cell def sky130_inv.

we can check the source of the PMOS is connected to the ground or not. and similarly we can check it for NMOS also.

Lab steps to create std cell layout and extract spice netlist
To extract the file from here, we have to write the command in tckon window. and the comand is extract all

image

Now let's go to this location from the terminal. it is exctracted.

image

we will use this .ext file to create the spice file to be use with our ngspice tool. for that we have apply the command ext2spice cthresh 0 rthresh 0. this will not create anything new. now again we have to type ext2spice command in tckon window.

image

so, now we are checking the location and at there spice file has been created.

image

let's see what inside the spice file by "vim sky130_inv.spice".

image

Sky130 Tech File Labs
Lab steps to create final SPICE deck using Sky130 tech
image

here, we can see the all details about the connectivity of the NMOS and PMOS and about the power supply also.

X0 is NMOS and X1 is PMOS and both's connectivity is shown as GATE DRAIN SUBSTATE SOURCE.

image

Now we have to include the PMOS and NMOS lib files. it is inside the libs folder in the vsdstdcellsdesign folder.

image

so, now we include this file in the terminal by .include ./libs/pshort.lib and .include ./libs/nshort.lib command.

And then set the supply voltage "VDD" to 3.3v by VDD VPWR 0 3.3V command. and similarly set the value of VSS also.

Now, we need to specify the input files. by Va A VGND PULSE(0V 3.3V 0 0.1ns 2ns 4ns).

Also add the command for the analysis like, .tran 1n 20n, .control , run,.endc,.end.

image

after running this file we get output of ngspice like this,

image

Now, ploting the graph here by command, plot y vs time a.

image

NOw if we increase the C3 value from 0.024ff to 2ff the graph will look like this, graph become more smoother.

image

Lab steps to characterize inverter using sky130 model files
Here, we have to find value of 4 parameters.

rise time
it is time taken to the output waveform to 20% value to 80% value.

image

so, rise time= (2.2489 - 2.1819)e-09 = 66.92 psec.

fall time
it is the time take by output for transition from 80% to 20%.

image

so, rise time= (4.09512 - 4.05264)e-09 = 42.51 psec.

propagation delay
it is the time difference between the 50% of input and 50% of the output.

image

so, propogation delay =(2.2106 - 2.15012)e-09 = 60.48 psec.

cell fall delay
it is time for output falling to 50% and input is rising to 50%.

image

so, cell fall delay =(4.07735 - 4.04988)e-09 = 27.47 psec.

We have successfully characterized our inverter. Our next objective is to create a lef file using the layout and we will plugin this lef file in the picorv32a core

Lab introduction to Magic tool options and DRC rules
To know more about the Magic DRC we can go to the website:- http://opencircuitdesign.com/magic/Technologyfiles/TheMagicTechnologyFileManual/DrcSection

Link to Google_Skywaters Design Rules: - https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html

For reference , we can use the github repo of Google-Skywater: - https://github.com/google/skywater-pdk

Lab introduction to Sky130 pdk's and steps to download labs
Follow the steps:

First go to the home directory.

To download the lab files for performing DRC corrections:

wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

To extract the lab files from the downloaded file:

tar xfz drc_tests.tgz

Then go inside the lab folder drc_tests.

To list all the directories, we can use the command ls -al.

To view the .magicrc file, we can use the command gvim .magicrc. This file serves as the startup script for magic and tells it where to find the technology file. The technology file is already available locally in the same directory, so we can make changes to it if needed.

To start the magic tool with better graphics, we can use the command magic -d XR &.

image

Content of .magicrc file by using command vi .magicrc

image

Lab introduction to Magic and steps to load Sky130 tech-rules
Use the command magic -d XR

to open the magic tool. Open the met3.mag file from the file menu. we will see different layouts with different DRC values, called rule numbers.

image

These rule number we can found at Google-Skywater documentation.

image

Now we will select the any layout area and check drc why in tckon.

image

Next, select a blank area and hover the mouse pointer over the metal3 contact icon. Press the p button and type 'pek' in the tkcon. Then execute the command cif see VIA2 in the tkcon tab.

we will see a bunch of black squares appear inside the area.

image

Lab exercise to fix poly.9 error in Sky130 tech-file
Now, we will open the poly.mag file in the magic tool with the helo of the command load poly.mag in the tkcon terminal.

image

Now consider the rule poly.9 then check the website for that particular rule.

image

image

To find the error, we can look at the sky130A.tech file which is present in the drc_tests directory. we can open this file with the text editor of Linux itself.

image

Search for 'poly.9' in the sky130A.tech file. It appears in both the POLY and uhrpoly sections, where the rules are not set properly. Add a change in both sections.

image

image

After making changes to the sky130A.tech file, click on Save and close the editor file.

Next, execute the command tech load sky130A.tech in the tkcon terminal. Then, run the drc check as shown below.

image

Lab exercise to implement poly resistor spacing to diff and tap
To correctly implement poly resistor spacing, we will need to make changes to the sky130A.tech file again.

image

Now will execute in Tkon after saving.

image

To check for errors, we can make a copy of the poly.9 model from the poly.mag file in the magic window.

image

To find the description of a DRC error, we can select the area with the error in the magic window and then run the command drc why in the tkcon terminal.

This will give a description of the error.

image

Lab challenge exercise to describe DRC error as geometrical construct
Now we will make some changes in sky130A.tech file which are as follows:

image

image

To find the nwell.6 model error, open the nwell.mag file in the magic tool. In the figure, the deep nwell is shown in yellow stripes and the nwell is shown in dotted green pattern.

image

This error can be seen at the site as well.

image

Lab challenge to find missing or incorrect rules and fix them
Now we will open the magic tool and execute the commands drc style drc(full) and drc check.

image

Day 4 - Pre-layout timing analysis and importance of good clock tree
Timing modeling using delay tables
Lab steps to convert grid info to track info
Now, we need to extract the '.lef' file from the '.mag' file to place it into the picorv32a flow.

There are certain guidelines to follow while making standard cells:

The input and output ports must lie on the intersection of the vertical and horizontal tracks.

The width of the standard cell should be an odd multiple of the track pitch, and the height should be an odd multiple of the track vertical pitch.

Now we can open the track file from pdk/sky130/libs.tech /openlane/sky130_fd_sc_hd/track.info to get more information on this.

image

The track is used during the routing stage and is essentially a trace of metal layers such as metal 1, metal 2, etc.

PNR is automated, so we need to specify where we want the routes to go. This specification is given by tracks. Each of the tracks is placed at (0.23, 0.46)um horizontally and (0.17, 0.34)um vertically for li1, metal 1, and metal 2 layers.

In the layout, the ports are on the li1 layer. To ensure that the ports are on the intersection of the tracks, we will need to convert the grid into the tracks.

To do this, we can first open the tracks file and then open the tkcon window and type the help grid command.

image

Then again will write command according to the track file required.

image

Now we can see that, the ports has been placed at the intersection of the tracks. But between the boundaries, 3 boxes are covered. so our second requirment also satisfies here.

Lab steps to convert magic layout to std cell LEF
Now, we will need to decide on the port name and its values. we can set the values for different ports, and for the power and ground port, we will need to make changes in the 'attach to layer' as Metal1.

image

After these parameters are set once, we are ready to extract the lef file from the mag

image

Now, we open this file in the magic by the command

magic -T sky130A.tech sky130_vsdinv.mag &

To extract the lef file we have to write the command in the tckon window as given below,

lef write

so it will create a lef file and we can check it in the vsdstdcellsdesign folder by using command ls -ltr.

image

Introduction to timing libs and steps to include new cell in synthesis
Now that the .lef file has been created, the next step is to plug this file into picorv32a. Before that, we will need to move the files to the src folder where all the design files are available at one location.

To do this, we can copy the file using the cp command.

image

this is how the library file looks like. We have different library files named as typical,slow ,fast.

image

Now we will copy it to the src folder

image

Here We need to modify the config.tcl file of picorv32a directory,

So open the config.tcl file of picorv32a directory and add the commands shown in below image :

image

OPENLANE :- Now we will go to the open lane directory and execute the docker command.

Will Execute the following commands in a line

./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]      
add_lefs -src $lefs
run_synthesis
image

image

image

Introduction to delay tables
Power Aware CTS:- If we make enable pin at logic '1' in the AND gate, then clock will propagate and if we make it 'logic 0' it will block the clock. Similarly in 'OR' gate if we make enable as 'logic 0' it will propagate and on making it 'logic 1' it will block the clock.

So the advantage of this blocking period is that we can save lot of power in clock tree.

image

Let's say we have a clock tree, The buffer present at the first input is driving the load of second two buffers. We have splited the buffer and in clock gating technique we have just swaped the buffer with and gate so now will all the other characteristic will be same or will get changed will see in coming steps.

image

Before swaping the buffer with gate we have made some assumptions which are follows

image

Assume c1=c2=c3=c4=25fF
Assume Cbuf1=Cbuf2=30fF
Total Cap at node 'A'=> 60fF
Total Cap at node 'B'=> 50fF
Total Cap at node 'C'=> 50fF
We have made some observations from here which are as follows

2 levels of buffering
At every level,each node driving same load
Identical buffer at same level
So the output capacitance of the buffer for the entire circuit is not constant, load at the output will be varying and since the load is varying so input transition is also varying.

So becuase of this variation in output and input we will have varity of delays so how to capture that delay we will see with delay tables

How delay tables are prepared?

For this purpose we have carried out the one buffer from the circuit and and seperately varying its input transition within some range of let's say 10ps-100ps than the output load will also vary so we willl characterise the delay and put the data in tabular format.

image

Delay table usage Part 1
Let's take the example for other buffers.

image

For practical example let's say we have the input transition of 40ps of buffer1 the output capacitance of this particular buffer is 60ff. The delay of the cell in this case is lies between x9-x10.

So the values which are not available in the delay table those are extrapolated from the given data so we can take the range in that case.

Delay table usage Part 2
Now we have to calculate the delay of buffer 2 and after that we can find the latency at the 4 clock end points.

Here input transition is common for both the buffers. now assuming that the transition is around the 60psec and load at both the buffers is 50fF. so it will give the delay of y15.

The total delay from input to the output is= x9' + y15.(here we are ignoring the delay of the wires). that means the skew at the any output point is zero.

If load is not same at the every nodes, the skew will not be the zero.

Lab steps to configure synthesis settings to fix slack and include vsdinv
We will try to modify the parameters of our cell by referring the README.md file in the configuration folder in openlane directory

The README.md file contains information about the parameters of the cell.

image

We will give the following commmands in the terminal in openlane directory

prep -design picorv32a -tag 01-04_12-54 -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

echo $::env(SYNTH_STRATEGY)

set ::env(SYNTH_STRATEGY) "DELAY 3"

echo $::env(SYNTH_BUFFERING)

echo $::env(SYNTH_SIZING)

set ::env(SYNTH_SIZING) 1

echo $::env(SYNTH_DRIVING_CELL)

run_synthesis

prep -design picorv32a -tag 01-04_12-54 -overwrite is used to overwrite the existing files with previous values of simulations.

After synthesis, we have observed that the slack is nagative.

wns(worst negative slack)= -23.89

tns(total negative slack)= -711.59.

image

image

Now run_synthesis we will see chip area has incresed and the value of slack has reduced.

Since synthesis of the picorv32a is successful, so we will run the floorplan using command run_floorplan

image

Since, we are getting the error so first again we have to do the synthesis using the commands mentioned earlier and then we will use following commands to do the floorplan,

init_floorplan

place_io

tap_decap_or

image

image

image

so now we are good to run the placement using command run_placement

image

Here placement is succesfull now without any error.

image

image

We will run the expand command in the tkcon window

image

Timing analysis with ideal clocks using openSTA
Setup timing analysis and introduction to flip-flop setup time
Timing analysis (with ideal clock):- Let's start the setup analysis with the ideal clock(single clock). specifications of the clock is

clock frequency =1 GHz

clock period =1 ns

Now will do the analysis between '0' and 'T' clock period. We sent at edge to the launch flop at '0' clock period and at T=1ns period the second edge reached to capture flop.

Let's say here we have combinatonal delay of theta and set up timing analysis says that this combinational delay should be less than the T for system to work properly.

image

Now let's open the capture flop and we will see some combinational circuit there it has several MOSFETs , several logics,resistances and capacitances inside it.Also have the time graph for this particular flop

image

When there is logic '0' or logic '1' of clock 1 the delay of MUX1 and MUX2 will restrict or effect the combinational delay requirement.

So there is some finite amount of time which is required to the D input to settle and this amount of time is reffered to as SET UP TIME.

Hence finite time 's' required before clk edge for 'D' to reach Qm.

So, we can write that the internal delay of the MUX1 = set up time(S).

So, now θ<T becomes θ<(T-S).

Introduction to clock jitter and uncertainty
So in Jitter the clock is being created by PLL(phase-locked loops) and the clk source is expected to sent the clk signal at exactly 0,T,2T,....But that clk source might or might ot be able to generate the clk exactly at 0 or any other certain time because of it's inbuilt variations that is called jitter. Jitter is refered as temporary variation of the clk pulse.

image

Let's consider this uncertantity time(US) in consideration. So, now equation will become θ<(T-S-US). Now assuming 'S'=0.01ns and 'US'=0.09ns. by taking this, Let's identify the timing path in our circuit stage 1 and stage 3 logic path has single clock.

Now,we have to identify the combinational path delay for the both logics.

image

image

Lab steps to configure OpenSTA for post-synth timing analysis
We have do STA on the picorv32a design which had timing violations.First we will run the synthesis using the following commands in openlane directory

docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_SIZING) 1
run_synthesis
image

Now we have to make a new pre_sta.conf file. We can do this by vim editor or in simple text editor also.

image

image

Now we will create a my_base.sdc file which will have the definitions of environment variables.

Now, we also need to create my_base.sdc file having the data shown in below image in openlane/designs/picorv32a/src directory

image

image

Now will go to the openlane directory in a new terminal and execute the sta pre_sta.conf command.

image

image

image

Lab steps to optimize synthesis to reduce setup violations
Now we will change the FANOUT parameter and again do the synthesis,

prep -design picorv32a -tag 02-04_05-27 -overwrite

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

set ::env(SYNTH_SIZING) 1

set ::env(SYNTH_MAX_FANOUT) 4

echo $::env(SYNTH_DRIVING_CELL)

run_synthesis

image

image

Now, run the sta pre_sta.conf command in a new terminal in openlane directory itself,

image

image

Lab steps to do basic timing ECO
OR gate which has a drive strength of 2 is driving 4 fanout.

image

So we have to replace this OR Gate with another OR Gate having Drive strength of 4 by executing the commands given below,

To Reports all the connections to a net

report_net -connections _11672_

To Check the command syntax

help replace_cell

To Replace the cell

replace_cell _14510_ sky130_fd_sc_hd__or3_4

To Generate the custom timing report

report_checks -fields {net cap slew input_pins} -digits 4

image

image

Now we can see theslack has been reduced from -23.89 to -23.51

image

In above case also OR gate which has a drive strength of 2 is driving 4 fanout.

So we have toreplace this OR Gate with another OR Gate having Drive strength of 4 by following these commands

To Reports all the connections to a net

report_net -connections _11675_

To Replace the cell

replace_cell _14514_ sky130_fd_sc_hd__or3_4

To Generate the custom timing report

report_checks -fields {net cap slew input_pins} -digits 4

image

image

Clock tree synthesis TritonCTS and signal integrity
Clock tree routing and buffering using H-Tree algorithm
Clock tree synthesis:- Let's connect clk1 to FF1 & FF2 of stage 1 and FF1 of stage 3 and FF2 of stage 4 with physical wire.

image

Now let's see what is the problem with this? Let's consider some physical distance from clk to FF1 and FF2 , so due to this t2>t1.

Skew= t2-t1, and skew should be 0ps

image

Previously we have build bad tree now we will try to modify that in a smarter way. Hrre clk will come in somewhere mid points with this clk will reach to every flip flop at almost same time. In the same way we will connect the clk2 with flip flops like midpoint manner.

image

Now will se clock tree synthesis(Buffering), Let's we have some clock route through which it has to reach to particular locations and clock end points and in the path many capacitance, resitors are there.

image

Because of the wire length we did not get the same wave form at ouput as input and bcz of RC networks , so to resolve this problem we use repeaters. The only difference between the repeaters we use for clock or for data path is that clock repeaters repeaters will have equal rise and fall time.

First step is we will remove the clock route and place 2 repeaters and allow the clock to go through this particular repeater, in this case whatever wave form is generated here will go to the output. So we can as many as repeaters we want to make the continuous flow of th clock till the output.

image

Crosstalk and clock net shielding
Clock Net Shielding:- Till now we have built the clk tree in such a fashion that the skew between the launch flop and capture flop is 0. Skew means the latency difference between clk ports of the flop pins. Clk net shielding is the critical net scene in the design. We take the particular clk net and shield it means we protect the clk from the outside world, it's like house for tha clk.

image

If we do not protect the clk then two types of problem we can face Glitch and Delta delay

Let's consider on of the clk net. So whenever there is switching activity happening at the aggraser because of the coupling capacitance between the wires and this capacitance is so strong that any activity happening at the aggraser will directly impact the net which is close.

image

The shielding is the technique, by which we can protect the net from these problems. In a shielding, we put wire between the either two wire where coupling capacitance is generate. This extra wire is grounded or connected to VDD.

We see the amount of delta delay because of the bump when we are switching from logic '1' to logic '0'. And skew is not anymore 0 here. So the impact of crosstalk delta delay is that it make the skew value non-zero.

image

By shielding we are breaking the coupling capacitance between the aggraser and victim. Shields don't switch.

Lab steps to run CTS using Triton
Now we need to replace the old netlist with newly generated netlist which has generated after reducing the slack. And then we will run floorplan , placement and CTS.

image

The image shown above has the netlist before making the modifications.

So, we need to make a copy of this old netlist and then we will add the newly generated netlist to be used in our openlane flow for further process.

So we will make the copy by following commands.

To go to the following location

cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/02-04_05-27/results/synthesis/

To List the contents

ls -ltr

To copy the netlist with particular name

cp picorv32a.synthesis.v picorv32a.synthesis_old.v

To List the contents

ls -ltr

image

Now we will do synthesis again then floorplan , placement and cts in the openlane directory itself by the following commands,

   prep -design picorv32a -tag 02-04_05-27 -overwrite
   set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
   add_lefs -src $lefs
   set ::env(SYNTH_STRATEGY) "DELAY 3"
   set ::env(SYNTH_SIZING) 1
   run_synthesis
   init_floorplan
   place_io
   tap_decap_or
   run_placement

   # Incase getting error will use this command
   unset ::env(LIB_CTS)

   run_cts
image

image

image

image

Now .cts file has created in the location as shown the image below,

image

Lab steps to verify CTS runs
OPENROAD To create a database in OPENROAD using LEF and TMP files, we can use the following commands:

First, make sure we are in the directory where the LEF and TMP files are located.

Then, enter the following command to start the OPENROAD tool,

openroad

Once you are in the OPENROAD tool, enter the following command to create the database,

To Read lef file

read_lef /openLANE_flow/designs/picorv32a/runs/02-04_05-27/tmp/merged.lef

To Read def file

read_def /openLANE_flow/designs/picorv32a/runs/02-04_05-27/results/cts/picorv32a.cts.def

To Create an OpenROAD database file named pico_cts.db

write_db pico_cts.db

Now we can see this database file is present in openlane directory.

Timing analysis with real clock using openSTA
Setup timing analysis using real clocks
Circuit tree looks little bit different than ideal clock with real clock here we have buffers,wires etc Clock reach to launch flop and capture flop through a set of buffers.So clock signal will not reach to the buffer at t=0 time due to these buffers. So the combinational circuit equation will be (θ+1+2)<(T+1+3+4). Initially it was θ<T.

image

Let's called "1+2"=∆1 and "1+3+4"=∆2 and (∆1-∆2)=skew

image

And here also, we have to consider the propogation skew (s) and uncertainty delay (US). so final equaltion becomes like, (θ+∆1)<(T+∆2-S-US).

we can also say that (θ+∆1)= data arrival time and (T+∆2-S-US)=data required time. If (Data required time)- (Data arrival time) = +ve then it is fine. If it is -Ve then it is called 'slack'.

Hold timing analysis:- It is littel bit different then setup timing analysis. here we are sending the first pulse to the both launch FLop and capture flop.

Hold condition state that, Hold time (H)< combinational delay (θ). So, (θ>H).

image

Hence, finite time 'H' required for 'Qm' to reach Q i.e., internal delay of mux2= hold time.

Now, if we add the real time clock, the equation will be change. now equation becomes (θ+∆1)>(H+∆2).

image

Hold timing analysis using real clocks
Combinational delay should be grater then the hold time of the capture flip flop. Once the clk reaches the launch flop it takes ariund 2buffer delay(∆1) and when it reaches to the capture flip flop it takes around 3 buffer delay(∆2). Uncertainity will be same for both the flip flops becaiuse clock applied to both the flops from the same edge only. Now let's add the uncertainity value to it.

image

Slack= Data ariival time - data required time. Slck either should be positive or 0. If slack goes to negative it is refer to as voilation

image

Let's identify the timing paths from design, with single clock

image

image image

Lab steps to analyze timing with real clocks using OpenSTA
Now we can execute the following commands,

To load the created db file in Openroad

read_db pico_cts.db

To read the netlist post CTS

read_verilog /openLANE_flow/designs/picorv32a/runs/02-04_05-27/results/synthesis/picorv32a.synthesis_cts.v

To read the library for design

read_liberty $::env(LIB_SYNTH_COMPLETE)

To link the design and library

link_design picorv32a

To read the custom sdc we have created

read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

To setting all clocks as propagated clocks

set_propagated_clock [all_clocks]

To Generate the custom timing report

report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

To exit from Openlane flow

exit

image

image

image

Lab steps to execute OpenSTA with right timing libraries and CTS assignment
To remove sky130_fd_sc_hd__clkbuf_1 from the list

set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

To check the current value of CTS_CLK_BUFFER_LIST

echo $::env(CTS_CLK_BUFFER_LIST)

To check the current value of CURRENT_DEF

echo $::env(CURRENT_DEF)

To set def as placement def

set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/02-04_05-27/results/placement/picorv32a.placement.def

To run cts

run_cts

To check the current value of CTS_CLK_BUFFER_LIST

echo $::env(CTS_CLK_BUFFER_LIST)

Lab steps to observe impact of bigger CTS buffers on setup and hold timing
Now we will follow the same commands we have used earlier to run OPENROAD,

openroad

read_lef /openLANE_flow/designs/picorv32a/runs/02-04_05-27/tmp/merged.lef

read_def /openLANE_flow/designs/picorv32a/runs/02-04_05-27/results/cts/picorv32a.cts.def

write_db pico_cts1.db

read_db pico_cts.db

read_verilog /openLANE_flow/designs/picorv32a/runs/02-04_05-27/results/synthesis/picorv32a.synthesis_cts.v

read_liberty $::env(LIB_SYNTH_COMPLETE)

link_design picorv32a

read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

set_propagated_clock [all_clocks]

report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

report_clock_skew -hold

report_clock_skew -setup

exit

image

image

image

image

Day 5 -Final step for RTL2GDS using tritinRoute and openSTA
Routing and design rule check (DRC)
Introduction to Maze Routing Ã�Â� LeeÃ�Â�s algorithm
So the next and final stage in the physical design is Routing and DRC.

Routing:- It is finding the best shortest possible connection between two end points with one point being the source and other point being the target and with less number of twist and turns.

Maze-Routing(Lee's Algorithm):- Therse should not be zig-zag lines of connections most of the connections should be in L shape or in Z shape. So according to algorithm first it create some grids and grids are routing at the backend. It's called as routing grid. There are some numbers of grids on this routig having some dimensions. SO here we are having two points one is 'Source' and the other is 'Target'. With the help of this routing grid algorithm has to find out the best possible way between them.

First step is algorithm tries to lable all of the grids surrounded. Only the adjacent horizontal and vertical grids are labeled not the digonal one as shown in the image below.

image

LeeÃ�Â�s Algorithm conclusion
Now we will lable the grids to the next integer untill we reach to the target. In the example we reached the target after integer 9.

![image](https://github.com/user-attachments/assets/c4d328d1-77f7-4840-8f89-ef7cd5a080bb)

SO now there are so many ways to reach to target from source but we have to choose the best shortest possible way to reach the target.And we need to avoid the zig-zag way better to cghoose 'L' shape routing'

![image](https://github.com/user-attachments/assets/3e7096c7-2172-442c-9ea7-ae70ea8bebf0)

Now take one more example for routing, and will follow the exact same step as follows in the above example.

![image](https://github.com/user-attachments/assets/9cea103c-50ad-4506-bfa6-3fce4ae18dd3)

![image](https://github.com/user-attachments/assets/e884ead7-c4d4-457c-b499-0b1b427dec10)

Design Rule Check
So in order to go to DRC we need to follow some steps which are called drc cleaning.

Let's take the example of the above circuit. Let's we have two parallel wires so the rule says that whenever we choose two wires there should be minimum distance between these two wires.

![image](https://github.com/user-attachments/assets/fa8e483d-a1b5-4160-90eb-160ca2a743d4)

Rule 1) Wire width:- Width of the wire should be minimum that derived from the optical wavelenth of lithography technique applied.

![image](https://github.com/user-attachments/assets/3ca628b6-45e5-4c17-91d5-e7cd5d9b34a0)

Rule 2) Wire Pitch:- The minimum pitch between two wire should be this much as shown in the figure below.

![image](https://github.com/user-attachments/assets/10524932-29ff-4468-b3dc-5af3347cfed6)

Rule 3) Wire Spacing:- The wire spacing between two wires should be as shown in the image below.

![image](https://github.com/user-attachments/assets/7027237f-9b9c-4031-96a1-acd9856da49f)

Let's take the other part for design rule check from the same example .

![image](https://github.com/user-attachments/assets/5bde6d4e-5277-46bf-9996-37b31596b7ab)

Solution of this signal short problem is take one of the wire and put it on the other metal layer. usually upper metal is wider than the lower metal.
![image](https://github.com/user-attachments/assets/a7ce342e-730a-4c7d-aff4-01818a89bea2)

After this solution, we add two new DRC rules should be check.

Rule 1) Via Width:- via width should be some minimum value.

![image](https://github.com/user-attachments/assets/421cc631-8af6-4387-84a5-d20dbd220323)

Rule 2) Via Spacing:- Via spacing should be minimum value.

![image](https://github.com/user-attachments/assets/196a7f65-7126-4353-8e02-c2e080e6622b)
After routing and DRC the next step is Parasitic extraction. Resistance and capacitance present on every wire should be extracted and use for further process.

![image](https://github.com/user-attachments/assets/5e395a88-ee3c-4ec8-97fb-9e043dc2b976)

Power Distribution Network and routing
Lab steps to build power distribution network
Command for the previous terminal are given bellow

docker

./flow.tcl -interactive

package require openlane 0.9

prep -design picorv32a -tag 30-03)20-42

echo $::env(CURRENT_DEF)

So, till here we have done CTS and now we are going to do the routing. but before routing we have to generate the PDN(power distribution network)file by using the command.

gen_pdn

![image](https://github.com/user-attachments/assets/d9821819-f444-43f5-adfe-c541ea9a9600)

![image](https://github.com/user-attachments/assets/397a8d87-aebd-4365-9150-8c869a19f5bf)

It seems like the net VGND displays the total number of nodes on the grid matrix, indicating that it has been successfully created.

The chip receives power from the VDD and GND pads, which then travels through the tracks and ultimately reaches the cells to power them.

Lab steps from power straps to std cell power
![image](https://github.com/user-attachments/assets/9abf69f0-a260-4549-a292-af36db67620f)

Here green color is representing the chip, and yellow, red and blue boxes are the I/O pins,power and ground pads respectively.

Power is transfered to the rings from the pads through the black dots shown in the image on the cross section points of the ring and pads.

We have vertical and horizontal tracks which ensures that the power is being transfered from the ring to chip this is shown by the red and blue color. This is how power planing works in physical design of any device.

Basics of global and detail routing and configure TritonRoute
The final step of physical design is Routing.

![image](https://github.com/user-attachments/assets/08670b8c-62a0-4556-8c1d-7eb61a72210d)

The usage of the def command in the image above is to indicate that the latest completed step was the generation of PDN.

The resulting file 17-pdn.def contains the information from cts.def as well as the power distribution network.

If one wants to learn about the various switches available for routing, they can refer to the README.md file located in the configuration folder of the OpenLANE directory.

![image](https://github.com/user-attachments/assets/95ad7920-7b1b-4be4-90d6-8afd44b1ce12)

By executing specific commands, we can determine the type of global and detailed routing that will be performed.

In case we want to change the routing type, we can use the set command followed by the parameter names mentioned in the routing section of the README.md file.

However, in this instance, the default routing types have been utilized.

![image](https://github.com/user-attachments/assets/174ffef7-203a-4d51-a9e7-aa47a9d0b6ec)

Now, we will proceed for routing process by using the command

run_routing

![image](https://github.com/user-attachments/assets/70ded05b-555a-4ca3-aeef-2d5bc0edbe30)

The total process of routing is devided into two part.

Fast route (Global route)
Detailed Route
![image](https://github.com/user-attachments/assets/34919a01-6006-4c7f-af1c-b431e297ff94)

In the Global route, the routing region is devided into the rectangular grids cells as shown in the figure above. And it is represented as cores 3D routing graph. Global route is done by FAST route engine.The detailed route is done by TritonRoute engine. A,B,C,D are four pins which we want to connect through routing. and this whole image of A,B,C,D shows the net.

TritonRoute Features
TritonRoute feature 1 - Honors pre-processed route guides
Performs initial detailed route
Hounors the prepocessed route guides(obtained after fast route)
![image](https://github.com/user-attachments/assets/b74978f4-1747-44cc-ae11-ab90d6ad7a51)

Requirements of preprocessed guides

Should have unit width

Should be in preferred direction

Assumes route guides for each net satisfy inter-guide connectivity
Two guides are connected if

They are on the same metal layer with touching edges.

They are on neighbouring metal layers with a non-zero vertically overlapped area.

Works on proposed MILP-based panel-routing scheme with intra-layer parallel and inter-layer sequential routing framework
TritonRoute Feature2 & 3 - Inter-guide connectivity and intra- & inter-layer routing
Each unconnected terminal i.e, pin of a standerd-cell instance should have its pin shape overlapped by a route guide.

![image](https://github.com/user-attachments/assets/26dc98b0-102b-4738-b2b6-c51a94e085b6)

Here we can see that black dots are pins of the cells and it is overlapped by route guide. if you have pins on the intersection of the vertical and horizontal tracks that will ensure that it will be overlapped by route guides.

Intra-layer parallel and inter-layer sequential panel routing

Intra layer means within the layers and inter layear means between the layers.

![image](https://github.com/user-attachments/assets/d37ce03b-13d0-4386-91a1-01eb6c793045)

In this figure we can see the 4 layers of metal. each of these layers are devided in to the "--" lines. lets focus on metal 2 layer. here we assume the routing direction vertical. These "--" lines are called pannels. each pannels assigns the routing guides. here we can see the blue arrows. here routing is heppenes in the even index. it means that intra layer parallel routing. first it is heppenes in the even index and the it will heppen in the odd index. but it is heppening in the parallel in this perticular layer.
![image](https://github.com/user-attachments/assets/bac47a2a-0413-4363-a094-a7ab27204b03)

(a) figure shows the parallel routing of panels on M2.

(b) figure, we can see the parallel routing of even panels on M3 and (c) shows the parallel routing of odd panels on M3.

TritonRoute method to handle connectivity
INPUTS:-LEF
OUTPUTS:-detailed routing solution with optimized wore-length and via count
CONSTRAINTS:-Route guide honouring, connectivity constraonts and design rukes
Now we have to defined the space where detailed routing take spaced.

Handling connectivity:-

![image](https://github.com/user-attachments/assets/3fd7395a-f251-4b46-948b-2415843aa9a1)

Access point(Ap):- An on-gride point on the metal layer of the route guide, and is used to connect to lower-layer segments, upper-layer segments, pins or IO ports.

Access point cluster (APC):- A union of all access points derived from same lower-layer segment,upper-layer guide, a pin or an IO port.

Here in the figure shown above, the illustration of access points:

(a)To a lower-layer segment

(b)To a pin shape

(c)To upper layer

Routing topology algorithm and final files list post-route
![image](https://github.com/user-attachments/assets/2f027f22-e20d-4ac5-988d-b3e0957ead44)

The algorithm requires the determination of the cost associated with each APC and the calculation of the minimum spanning tree between the APCs to find the optimal points between two APCs.

The next step involves post-routing STA analysis, which requires the extraction of parasitic effects (SPEF).

Since OpenLANE does not have a SPEF extraction tool, this process needs to be done outside of OpenLANE.

The resulting .spef file can be located in the routing folder under the results folder.



![image](https://github.com/user-attachments/assets/b3b88ffb-a493-4fac-9da3-f409d6507333)

![image](https://github.com/user-attachments/assets/9bdfdd2d-7df8-4e2f-ad1d-61caa9544540)

This is the final generated layout.

![image](https://github.com/user-attachments/assets/859e8671-ad66-4cb8-a260-e9665bfe9ffc)

