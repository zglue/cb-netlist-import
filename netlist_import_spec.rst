SPICE Netlist Format Specification
**********************************

Overview
========

To properly define a ZIP chip schematic, there are certain specifications that need to be met. ChipBuilder abstracts these for you; however, to create said design with standard schematic capture tools, it becomes necessary to specify them in detail for users to follow. The following specification focuses on the format that a generated SPICE netlist needs to adhere to for ChipBuilder to properly port a ZIP chip schematic. It is also worth noting that this document is only one portion of the ``Netlist Import specification``. 

As mentioned in the ``README.rst`` file, the netlist format specification that follows is valid SPICE format, and is simulatable with the correct simulation engine. With that said, this document does not read as a SPICE format guide. Instead, it focuses on how the different components that make up a ZIP are represented in SPICE to successfully generate, and import, a netlist into a ChipBuilder system. A netlist following this specification is capable of describing all the details that make up an internal ZIP schematic.


Netlist Components
==================

Any ChipBuilder importable netlist is comprised of a combination of the following:

* A metadata header
* A SmartFabric IP block
* Chiplets
* Bondpad labels
* Programmable resistors


Metadata Header
---------------

This header is optional, but it's useful for documentation purposes. Include information such as design name, date and time of creation, authors, etc.. These fields are comment lines in SPICE, which would need to be preceded by an asterisk ``*``.


Generic Format
--------------

The description of the schematic itself is comprised of models and model instances. Some of these models are inherent to SPICE (e.g. resistors), some are specified by zGlue, others have to be defined (e.g. chiplets). These models are defined with the ``.SUBCKT`` declaration. Additionally, a ``.PININFO`` comment line is used to map component pin numbers with their respective pin names, which provide the information necessary for ChipBuilder to validate components placed in the schematic. On the other hand, part instances represent the logical connections between the specific components placed on the schematic. To define an instance, the reference designator for each part is followed by a net name list representing each component pin described by their assigned model. Below is a generalized format for both of these declarations.

Subcircuit Model::

    .SUBCKT <ModelName> <PinNumber[1]> <PinNumber[2]>... <PinNumber[n]>
    *.PININFO <PinNumber[1]>:<PinName[1]> <PinNumber[2]>:<PinName[2]>... <PinNumber[n]>:<PinName[n]>
    .END

Part Instance::

    <PartReference> <NetName[1]> <NetName[2]>... <NetName[N]> <ModelName>

.. note::

    Net names don't need to be ordered alphanumerically; however, the pin number order used in the subcircuit model needs to be conserved in the part instance.


SmartFabric IP Block
--------------------

Every ZIP chip design uses the SmartFabric active silicon interposer, which means that every schematic should contain one, and only one, instance of this schematic symbol. The pin numbers and names for this component have to follow the ``SmartFabric IP`` definition described in the ``ChipBuilder Symbols Specification``.

Subcircuit model name: ``SmartFabric``

Part Instance::

    S1 <NetName[1]> <NetName[2]>... <NetName[N]> SmartFabric

.. note::
    
    Since there can only be one instance of this symbol, the reference designator is expected to be ``S1``.


Chiplets
--------

The requirements for chiplets are described in the ``Schematic Guidelines`` sections of the ``ChipBuilder Symbols Specification``. The model names for these components should be their respective part numbers, and the reference designator has to start with 'U'.

Subcircuit model name: ``<BasePartNumber>``

Part Instance::

    U? <NetName[1]> <NetName[2]>... <NetName[N]> <BasePartNumber>


Bondpad Labels
--------------

Bondpad labels are a ChipBuilder specific concept. These labels simply identify ZIP internal nets that need to be assigned to a specific ZIP package pin as an IO. The model and instance definitions are described below.

Subcircuit model::

    .SUBCKT BONDPAD 1
    *.PININFO 1:1
    .END

Part Instance::

    BP? <NetName[1]> BONDPAD


Programmable Resistors
----------------------

The programmable resistors are modelled as standard SPICE resistors. This happens organically since the part reference for the resistor already implies a simple resistor model. For this reason, a subcircuit model for a resistor is not necessary. Follow the description below to define a programmable resistor instance. 

Part Instance::

    RP? <NetName[1]> <NetName[2]> <ResistanceValue>

.. note::

    The 'P' in ``RP?`` denotes the resistor as programmable. This identifies the resistor as an integrated programmable resistor in the SmartFabric active interposer. Although any resistor instance starting with 'R' would be imported correctly, it should be common practice to differentiate the resistors from discrete components to give a physical sense of where these passive exist.

