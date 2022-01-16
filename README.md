# IOb-MEM

IOb-MEM is a repository of memory Verilog descriptions. These descriptions are
meant to be included in larger designs, freeing the user from the many
idiosyncrasies of the various design tools that are unable to understand plain
Verilog code, and only accept descriptions that follow their own recipes.

## Use in IOb-SoC

IOb-MEM is used in the [IOb-SoC](https://github.com/IObundle/iob-soc) RISC-V
System on Chip template and in many other [IObundle, Lda](https://iobundle.com)
projects.

Each module is provided with a Verilog testbench and a Makefile segment called
hardware.mk. The hardware.mk Makefile segment may include the hardware.mk
segments that are used in other modules that may be included in the module of
interest. The hardware.mk Makefile segment sets the variables used in IObundle
projects, such as the MODULES variable, which contain a list of all modules used
in the project. Another variable of interest contains the directory path of the
module. The list of Verilog sources is added to the variable VSRC. Finally, the
required Verilog macros are added to the DEFINE variable so that the module is
used with its definitions in simulation and synthesis.


## Simulation

Each memory module can be simulated using the free of charge and open-source
Icarus Verilog simulator by using the accompanying Makefile. Other simulators
can be used. To simulate a module using the Icarus Verilog simulator, type

```
make sim MODULE_DIR=$(MEM_HW_DIR)/ram/<module>
```

where MODULE\_DIR is a variable that points to the module to be simulated. By
default MODULE\_DIR=$(MEM_HW_DIR)/ram/sp\_ram, pointing to the single-port RAM
module `sp\_ram`. To clean the simulation generated artefacts placed in the root
directory, type

```
make clean 
```


## Synthesis

Some synthesis tools are able to infer memory blocks from their Verilog
descriptions such as FPGA synthesis tools. In this case simply instantiate the
module and list all its source file dependencies. Some of these sources may
contain `ifdef` statements to support specific FPGAs, so the user should inspect
these files to know if some of those macros need to be set.


## Use in ASICs

Unfortunately, few ASIC synthesis tools are capable of inferring memory blocks
from their Verilog descriptions. Instead, hard macros of the modules must be
generated by a memory compiler tool and an empty Verilog wrapper must be
instantiated in the code to include the memory modules. A Python tool called
memwrapper.py can be found in `software/python`directory, which generates
suitable memory wrappers for a few memory compilers; feel free to contribute to
this program by providing additional cases to support other memory compilers and
memory blocks.
