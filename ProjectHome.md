# Introduction #
This project aims at creating an Open Source [Digital Sampling Oscilloscope (DSO)](http://en.wikipedia.org/wiki/Digital_Sampling_Oscilloscope#Digital_sampling_oscilloscopes).

The target is to design a 2-channel, 100 MS/s sampling rate, 8-bit resolution portable device.

Although there are numerous DSO projects floating around on the Net, none of them is suitable because:
  * it is not meeting the above characteristics
  * it actually felt below the announced expectations
  * it is non-free

Based on this observation, this project was started with several goals in mind:
  * propose an Open Source DSO with useful performance at a reasonable price
  * in its bare bone version, provide a cheap analog front-end for [FPGA](http://en.wikipedia.org/wiki/Fpga) or [DSP](http://en.wikipedia.org/wiki/Digital_signal_processor)-based circuits
  * expose the project internals for learning high-speed [analogue electronic](http://en.wikipedia.org/wiki/Analogue_electronics) design and [FPGA](http://en.wikipedia.org/wiki/Fpga)/[Soft-Core](http://en.wikipedia.org/wiki/Soft_processor) co-design
  * eventually get familiar with the industrialization process of a complex electronic device

# License #
Unless otherwise stated, all the documents pertaining to this project are placed under the [Attribution-ShareAlike 3.0 Unported (CC BY-SA 3.0) License](http://creativecommons.org/licenses/by-sa/3.0/), and all sources are placed under the [GNU General Public License (GPL)](http://www.gnu.org/licenses/gpl.html).

# Documentation #
We are trying to set up a [draft specification](Specification.md) and we started a [block diagram](BlockDiagram.md).

# Hardware #
We decided to work concurrently on the analog front-end and the FPGA design, using a simulating high-speed signal generator.

# Software #
Nothing done yet.

# Links #
We will try to compile a useful list of links to documents related to the project:
  * [application notes](ApplicationNotes.md)
  * [books](Books.md)
  * [datasheets](Datasheets.md)
  * [similar projects](SimilarProjects.md)
  * [tutorials](Tutorials.md)

# TODO #
Everything!

# How to Help? #
As it is today, this project is a work in progress, and you are more than welcome **[to contribute](HowToContributeToTheProject.md)** to it!