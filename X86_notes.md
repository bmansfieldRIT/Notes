## x86 Instruction Set Architecture

#### About x86:
* x86 is a family of instructions et architectures
* based on Intel 8086 CPU and Intel 8088 variant
* 8086 introduced in 1978 as a 16 bit extension of 8 bit 8080 microprocessor w/ memory segmentation as solution for addressing more memory than can be covered by a plain 16 bit dress
* x86 is the name because of the several successor microprocessors that end in 86, (80186, 80286, etc)
* maintains mostly consistent backwards compatibility with older versions
* architecture has been implemented in Intel, Cyrix, AMD, VIA processors
* there are open source implementations of x86
* only Intel, AMD, and VIA hold architectural licenses, and are producing modern 64 bit designs
* majority of personal computers and laptops are x86, while smartphones and tablets are mostly ARM
* x86 dominates compute intensive workstation and cloud computing segments

#### Overview:
* 80386 instruction set is sort of a lowest common denominator for modern operating systems
* x86 is uncommon in embedded systems, home appliances/toys
* many attempts have been made to replace ‘inelegant’ x86 arch, but continuous refinements, feature adds, make replacement very difficult in many segments
* AMD releases 64 bit arch, calls it x86-64
* Windows designates 32 bit version as x86 and 64 bit version and x64

#### Chronology:
* 1978 - first intel x86 microprocessors 8086, 8088
* 1982 - hardware for fast address calculations, multiplication, division
* 1985 - 32 bit instruction set, 80386
* 1993 - superscalar, Pentium, Pentium MMX
* 1995 - out of order, register renaming, speculative execution, Pentium Pro
* 2000 - deeply pipelined, 20 pipeline stages, hyper threading pentium 4
* 2006 - Intel 64 processors, low power, multi core, Intel Core 2
* 2007 - Monolithic quad core, AMD Phenom
* 2008 - Intel core i3, i5, i7
* 2011 - 2nd and 3rd gen Intel core i3, i5, i7 (sandy bridge, ivy bridge)
* 2013 - 4th and 5th gen intel core i3, i5, i7 (Haswell, Broadwell)
* 2015 - 6th, 7th, 8th gen intel core i3, i5, i7 (Skylake, Kaby lake, Coffee Lake)
* 2017 - AMD Ryzen 3/5/7, Zen and Zen+

#### Basic properties of x86:
* variable instruction length
* primarily CISC
* emphasis on backwards compatibility
* site addressing is enabled, words are stored in memory with little endian byte order
* memory access to unaligned addresses is allowed for all valid word sites
* largest native size for integer arithmetic and mem addresses is 16, 32, 64 bit
* typical instructions are 2/3 bytes in length, some longer some single byte
* most registers are ecxpressed in opcodes using 3/4 bits
* relatively small number of registers, register relative addressing important
* floating point processor instructions can work in parallel on 1/2 128 bit words, bitwise ops only


#### Implementations:
* during execution, instructions are split with decoding  steps into smaller pieces called micro operations
* then handed to a control unit to buffer and schedule them to be executed partly in parallel by one of several specialized execution units
* thus, modern x86 designs are pipelined, superscalar
* can also turn common sequences of instructions into more complex opcodes (compare->jump)

#### 16 bit Registers:
* original 8086 have fourteen 16 bit registers
* 4 general purpose registers (GPR), AX, BX, CX, DX
* each may have an additional purpose: only CX can be used as a counter in a loop instruction
* each can be accessed as two separate bytes (BH gets BX high byte, BL gets BX low byte)
* SP points to top of stack
* BP points to some other place in stack, typically above local variables
* SI, DI, BX, BP are address registers, may be used for array indexing
* segment registers CS, DS, SS, ES used to form a memory address
* FLAGS register contains carry flag, overflow flag, zero flag, etc
* IP instruction pointer, points to next instruction that will be fetched from mem then executed

#### 32 bit registers:
* with advent of 80386 processor, general purpose registers, bad registers, index registers, instruction pointer, flags register (but not segment registers) expanded to 32 bits
* represented by E prefix (EAX corresponds to 32 bit AX, AX corresponds to lowest 16 bits of EAX)
* new segment registers (FS, GS) added
* with 80486, floating point processing unit is integrated on chip

#### 64 bit registers:
* R prefix identifies 64 bit registers (RAX, RSI)
* eight additional 64 bit general registers introduced

#### Register “Purposes”:
* AL/AH/AX/EAX/RAX - accumulator
* BX+ base index for use with arrays
* CX+ counter for use with loops and strings
* DX+ extend precision of accumulator
* SI+ source index for string operations
* DI+ destination index for string ops
* SP+ stack pointer for top address of the stack
* BP+ stack base pointer for holding the address of the current stack frame
* IP+ instruction pointer, holds program counter, the current instructions address
* segment registers:
* CS code
* DS data
* SS stack
* ES, FS, GS extra data

* some instructions execute more efficiently when using registers for their defined purpose
