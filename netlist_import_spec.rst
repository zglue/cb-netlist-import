SPICE Netlist Format Specification
**********************************

Overview
========


Implementation
==============



Netlist Components Models
=========================

As mentioned previously, the netlist format specification that follows is valid SPICE format, and is simulatable with a SPICE engine. It focuses on 

* SubCircuit Models
* Part Instances


MetaData Header
---------------

This header is optional, but it's useful for documentation purposes. Include information such as design name, date and time of creation, authors, etc..


Generic Format
--------------

Subcircuit Model::

    .SUBCKT <ModelName> <PinNumber[1]> <PinNumber[2]>... <PinNumber[n]>
    *.PININFO <PinNumber[1]>:<PinName[1]> <PinNumber[2]>:<PinName[2]>... <PinNumber[n]><PinName[n]>
    .END

Part Instance::

    <PartReference> <NetName[1]> <NetName[2]>... <NetName[N]> <ModelName>

.. note::

    Net names don't need to be ordered alphanumerically; however, the pin number order used in the subcircuit model needs to be conserved in the part instance.


SmartFabric IP
--------------

Subcircuit model name: ``SmartFabric``

Part Instance::

    S1 <NetName[1]> <NetName[2]>... <NetName[N]> SmartFabric


Chiplets
--------

Subcircuit model name: ``<BasePartNumber>``

Part Instance::

    U? <NetName[1]> <NetName[2]>... <NetName[N]> <BasePartNumber>

Bondpad Label
-------------

Subcircuit model::

    .SUBCKT BONDPAD 1
    *.PININFO 1:1
    .END

Part Instance::

    BP? <NetName[1]> BONDPAD


Programmable Resistors
----------------------

The programmable resistors are modelled as standard SPICE resistors. This happens organically since the part reference for the resistor already implies a simple resistor model. For this reason, a subcircuit model for a resistor is not necessary.

Part Instance::

    RP? <NetName[1]> <NetName[2]> <ResistanceValue>

