ChipBuilder Symbols Specification
*********************************

Overview
========

To properly define a ZIP chip schematic, there are certain specifications that need to be met. ChipBuilder abstracts these for you; however, to create said design with standard schematic capture tools, it becomes necessary to specify them in detail for users to follow. The following document focuses on describing the ChipBuilder symbols that a user would need to create their schematic, together with certain schematic guidelines the different components need to adhere to. It is also worth noting that this document is only one portion of the "Netlist Import" specification.

All schematic capture tools have the ability to create part libraries. It should be common practice to create a dedicated library for the ChipBuilder symbols described below; however, it is possible to define these symbols in any component library as long as it follows the definitions specified in this document.


Schematic Guidelines
====================

The following guidelines describe which components can or can't be integrated in a ZIP chip schematic. Any special considerations will be described as well.

Chiplets
--------

Chiplets are the components that get soldered on top of the SmartFabric interposer. Note that not all ICs are chiplets. As defined by zGlue, a chiplet is a pre-fabricated IC that is validated by zGlue to be part of a ZIP system. However, if a given IC is part of the `Chiplet Store <https://chipletstore.zglue.com/products/chipletstore>`_, it guarantees that the requirements are met. Alternatively, an IC needs to meet the following criteria:

* The IC needs to be in a CSP or BGA package. In other words, the package needs to have solder balls, not pads (e.g. LGA).
* All the ICs need to fit on top of the SmartFabric interposer

If these requirements are not met, the IC would need to be placed on the PCB, or integrated in a custom ZIP substrate.


Passives
--------

Discrete passive components cannot be integrated in the active silicon interposer; however, they can be integrated in a custom ZIP substrate. Many programmable resistors are already integrated inside the SmartFabric active interposer, which can be used to create a variety of resistor networks.


Library Symbols
===============

The following schematic symbols should be created as part of a custom library with ChipBuilder components. Making dedicated symbols for these components will simplify maintenance as this specification evolves with new features.

SmartFabric IP
--------------

The SmartFabric silicon interposer contains a variety of IP blocks that can be used to eliminate BOM from your system. These IPs are common to all ZIP chip designs, which means that every schematic should contain only one instance of this block. This symbol is described below.

* Part Designatior: ``S?``
* Part Value:       ``SmartFabric``
* Part Pins:

============  ==============
SmartFabric Pin Table
----------------------------
 Pin Number     Pin Name 
============  ==============
    1           ZIP_CS
    2           ZIP_SCLK
    3           ZIP_MOSI
    4           ZIP_MISO
    5           ZIP_SCL
    6           ZIP_SDA
    7           FAULT_INT
    8           MCU_RST
    9           ULPM_WAKE
    10          VBATH
    11          SYSLDO_OUT
    12          ZIP_EN_H
    13          ZIP_EN_OUT
    14          ZIP_EN_L
    15          VBATL
    16          VBATL_SW
    17          VX
    18          BOOST_OUT
    19          BOOST_GND
    20          LDOx_IN
    21          LDO1_OUT
    22          LDO2_OUT
    23          LDO3_OUT
    24          VDDANA
    25          VDDIO
    26          VRAIL1
    27          VRAIL2
    28          VBAT_SNS
    29          GND
    30          GPIO_EXP_INT
    31          GPIO0
    32          GPIO1
    33          GPIO2
    34          GPIO3
    35          GPIO4
    36          GPIO5
    37          GPIO6
    38          GPIO7
    39          ISINK1
    40          ISINK2
    41          ISINK3
    42          VCCA1
    43          VCCA2
    44          VCCA3
    45          VCCB1
    46          VCCB2
    47          VCCB3
    48          LS1_A1
    49          LS1_A2
    50          LS1_A3
    51          LS1_A4
    52          LS1_A5
    53          LS1_A6
    54          LS1_A7
    55          LS1_A8
    56          LS1_B1
    57          LS1_B2
    58          LS1_B3
    59          LS1_B4
    60          LS1_B5
    61          LS1_B6
    62          LS1_B7
    63          LS1_B8
    64          LS2_A1
    65          LS2_A2
    66          LS2_A3
    67          LS2_A4
    68          LS2_A5
    69          LS2_A6
    70          LS2_A7
    71          LS2_A8
    72          LS2_B1
    73          LS2_B2
    74          LS2_B3
    75          LS2_B4
    76          LS2_B5
    77          LS2_B6
    78          LS2_B7
    79          LS2_B8
    80          LS3_A1
    81          LS3_A2
    82          LS3_A3
    83          LS3_A4
    84          LS3_A5
    85          LS3_A6
    86          LS3_A7
    87          LS3_A8
    88          LS3_B1
    89          LS3_B2
    90          LS3_B3
    91          LS3_B4
    92          LS3_B5
    93          LS3_B6
    94          LS3_B7
    95          LS3_B8
============  ==============


Bondpad Label
-------------

Bondpad labels are not physical components, but their presence implies a physical routing requirement for the net it's connected to. A bondpad label identifies a net as a ZIP chip IO, which means that it will be required for that net to be routed to a package pin. Exactly how it's routed is specified in the ChipBuilder system once the netlist is imported. Follow the specification below for the bondpad label symbol.

* Part Designatior: ``BP?``
* Part Value:       ``BONDPAD``
* Part Pins:

============  ==============
Bondpad label Pin Table
----------------------------
 Pin Number     Pin Name 
============  ==============
    1           1
============  ==============


Programmable Resistor
---------------------

To differentiate an integrated programmable resistor on the SmartFabric interposer from discrete passives, it's recommended to create a separate symbol for it. Follow the specification below to create a symbol for this special resistor. 

* Part Designatior: ``RP?``
* Part Value:       ``<ResistanceValue>``
* Part Pins:

============  ==============
Resistor Pin Table
----------------------------
 Pin Number     Pin Name 
============  ==============
    1           1
    2           2
============  ==============

.. note::

    Alternatively, it is possible to manually modify the part designator of an already-existing resistor symbol after placing it on the schematic, but future features may include other properties to further specify these programmable resistors. As described in the ``Library Symbols`` section, making a dedicated part on a library is recommended.
