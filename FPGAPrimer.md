# Introduction #

FPGA development certainly appears as a kind of "black magic" to common mortals, partly because it is often confused with the full-fledged microelectronic design process.

Of course, there is no wizardry behind it. But the apparent complexity of the available FPGA datasheets, development boards and software tools make things rather obscure at first sight.

Fortunately, it is possible to make a parallel between the FPGA development process and the common software development tasks: at least, this is what this FPGA Primer will try to show.

Much like software, the FPGA development process is made up of several layers of increased abstraction level, going from the lowest transistor level to the highest application level.

# Level 0: the transistor #
Let's clear things out: when dealing with FPGAs, you are not laying out micro transistors on the silicium die! This task is only necessary when designing a "custom chip", and this is were "black magic" lies...

There is no requirement either to re-arrange the connections between these transistors: this step is only required when designing an "[ASIC](http://en.wikipedia.org/wiki/Application-specific_integrated_circuit)".

Instead, while dealing with FPGAs, the basic unit you work with is not the transistor, but rather a larger "macrocell".

Thus, the FPGA design goal is not to re-arrange the transistor layout (, nor it is to route the connections between these transistors, and not even to route the connections between the macrocells: all this is already engraved in the silicium die, and there is nothing you can change to it.

However, provisions has been made on the FPGA chip to have software addressable switches that can modify temporarily the existing connections between macrocells for a given purpose: this is how the FPGA will allow to build dedicated functions.

# Level 1: the logic gate #
FPGAs are digital in essence: they do not work with analog signals but with digital values, using [logical gates](http://en.wikipedia.org/wiki/Logic_gate) and [registers](http://en.wikipedia.org/wiki/Hardware_register) to provide a predictable behavior when performing a given task (ie. not subject to the random noise or accuracy problems of analog signals).

These gates and registers can be combined at heart's desire using [combinational](http://en.wikipedia.org/wiki/Combinational_logic) and [sequential](http://en.wikipedia.org/wiki/Sequential_logic) logic to perform very complex tasks. This vast subject will not be covered here, as these are out of the scope of this Primer.

# Level 2: the "macrocell" #
Logic gate combinations are nice, but what is more interesting though, is that by using a small SRAM, you can get all the Boolean logic operation into one single reconfigurable schematic (much like a "truth table"): let's say you have 4 binary signals. You can use these signals as the address lines of a 16x1bit SRAM, whose output can thus be "programmed" to be whatever Boolean logic function you want! In FPGA devices, this SRAM is called the "LUT" ("[Look-Up Table](http://en.wikipedia.org/wiki/Lookup_table#Hardware_LUTs)"), and this take care of all Boolean logic functions.

If you now consider the classic "[adder](http://en.wikipedia.org/wiki/Adder_(electronics))" circuit with its special [look-ahead carry generator](http://en.wikipedia.org/wiki/Lookahead_Carry_Unit) (in case you don't know yet, there is a combinational way to predict the carry in advance, so you don’t accumulate the delay when you add more bits to the binary words you manipulate), you can also perform all boolean arithmetic operation at once.

Right now, we are only able to deal with all combinational operations: we still lack flip-flops, so we will then be able to get all sequential operations, too!

So that's what a basic "FPGA macrocell" is made of:
  * 1 (or more often 2) LUTs for the logic operations (this LUT can also be used as a standard 16x1 "embedded RAM" memory block)
  * 1 adder with carry look-ahead, most of the time combined with a barrel-shifter circuitry to perform the arithmetic operations
  * 2 flip-flops for the sequential operations
  * some muxes to configure the data path programmatically

Imagine this basic macrocell repeated a few 10 000 or 100 000 times, and you get an FPGA!

There are however differences between FPGA manufacturers: each one has its own _better_ "macrocell" and "interconnection" strategy over the others (or this is what they pretend!). In fact, this is not important to us, as the FPGAs are never programmed at this low level: this is becoming about the same debate as between CPU families nowadays.

What we are interested in is in fact only the required "mux config" for all the macrocells, so the whole circuit performs the required operations. This is very close to the [assembly](http://en.wikipedia.org/wiki/Machine_language) "machine code" for a given CPU. This configuration must be fed into the FPGA every time it is powered on, as the FPGA has no builtin Flash or ROM memory: this would require too much valuable real-estate, as a Flash cell is far more cumbersome than a macrocell. This is why you have to provide this configuration bitstream either through a JTAG-style interface, or to store it into an intermediate serial Flash memory, that the FPGA is able to fetch automatically at boot-time. Again here, nobody really care about this bitstream, as it is both manufacturer and device-dependant. You can still get a view of the exact "mux config" in the so-called "technological representation" of each manufacturer's IDE (not easy to deal with, but sometimes, it gives you some hints why you generated a "monster").

# Level 3: the "Register Transfer Level" #
Next level up is matching the "assembly language" in the general-purpose CPUs: this is called the "RTL representation" (for "[Register Transfer Level](http://en.wikipedia.org/wiki/Register_transfer_level)"). Here, each vendor IDE is able to get you a schematic with recognizable combinational logic and flip-flops. The RTL names come just from this: each complex arbitrary circuit can be described as combinational operations between flip-flops (also named "registers"). As the flip-flops are pretty "static" (as they are not really configurable), the main job here is to be able to describe the combinational logic between them, so the name.

# Level 4: the "Hardware Description Language" #
Eventually, we arrive at the level where you actually "write" something! This is the HDL "[Hardware Description Language](http://en.wikipedia.org/wiki/Hardware_description_language)" level, which is similar to the high-level language used in software programming, like C, C++ or whatever you prefer! And this is exactly what it is: a high-level language with entities like loops, tests, variables, functions, etc. The main difference is that you deal with a lot of operations that take place in parallel, much like when you program using threads or tasks. You may wonder why use a language, rather than a "good old schematic"? Well, past a given complexity, schematics are not so helpful, and you spend most of you time making them readable. Plus, they cannot be checked for but trivial errors. A language on the other side, can be checked and validated by a compiler. The 2 available languages are Verilog (C-syntax) and VHDL (Pascal or Ada syntax). The manufacturer's IDE contain such compilers that will translate these languages into RTL.

# Conclusion #

Doing the travel top-down, working with FPGAs is very similar to working with software:
  * you use an editor to write high-level source code
  * a compiler will translate the source HDL into "assembly-level" RTL description
  * an "assembler" will translate the RTL into specific bytecodes
  * a "fitter" will place and interconnect these (like the linker...) into macrocells
  * the bitstream is sent to the FPGA at runtime (this is the "Field-Programmable" in FPGA!) or stored in a serial Flash and loaded at boot time