## Directives {#directives}

| Directive | Meanings |
| :--- | :--- |
|  **Output controls** |
| .cpu | Select CPU type. Available values are: 8080, Z80, 6502, M6800, CDP1802, M6809, C65816 |
| .engine | Controls machine type for emulation \(only in online[ASM80](https://www.asm80.com/)\). Available values are: PMI, PMD, JPR, KIM, SBCZ80, SBC6502, SBC09, ZXS and CPM |
| .pragma sna | Makes SNA file instead of HEX \(only for Z80\) |
| .pragma tap | Makes TAP file instead of HEX \(only for Z80\) |
| .pragma prg | Makes C64's PRG file instead of HEX \(only for 6502\) \[[read more](https://www.uelectronics.info/2015/04/10/asm80-news-cpm-c64-etc/)\]  .PRAGMA PRG ;says “make .PRG instead of .HEX” .ORG $0810 ;or higher .ENT $ ;for “enter here” |
| .pragma com | Makes CP/M COM file instead of HEX \(only for Z80/8080\) \[[read more](https://www.uelectronics.info/2015/04/10/asm80-news-cpm-c64-etc/)\] |
| .pragma html | Makes HTML listing \(instead of LPT\) |
|  **Data definition** |
| db \(aliases: defb, fcb\) | Define Byte. The argument is a comma separated list of string literals or numeric expressions. The string literals are inserted in the object code, and the result of the numeric expression is inserted as a single byte, truncating it if needed. You can use DUP for entering N same values: DB 10 DUP \(123\) means "10 times value 123" |
| dw \(aliases: defw, fdb\) | Define Word. The argument is a comma separated list of numeric expressions. Each numeric expression is evaluated as a two byte word and the result inserted in the proper "endianity". You can use DUP for entering N same values: DW 10 DUP \(123\) means "10 times value 123" |
| ds \(aliases: defm, defs, rmb\) | Define Space. Take one argument, which is the amount of space to define, in bytes. |
| fill_value, length_ | Fill memory with a value. Take two arguments, the first is a value, the second is length of filled block \(byte count\). |
| bsz_length_\(alias: zmb\) | Fill memory with a given count of zeros. |
| .include_filename_ | Include a file. The file is readed and the result is the same as if the file were copied in the current file instead of the INCLUDE line. The file included may contain INCLUDE directives, and so on. INCLUDE directives are processed before the assembly phases, so the use of IF directives to conditionally include different files is not allowed. |
| **Code control** |
| org_addr_ | ORiGin. Establishes the origin position where to place generated code. Several ORG directives can be used in the same program, but if the result is that code generated overwrites previous, the result is undefined. |
| .ent_addr_ | ENTer point for debugging. I.e.**.ent $** |
| .align_N_ | The .align directive causes the next data generated to be aligned modulo N bytes. |
| .phase_addr_ | Continue to produce code and data for loading at the current address but assemble instructions and define labels as if they originated at the given address. Useful when producing code that will be copied to a different location before being executed. |
| .dephase | End phase block. |
|  **Preprocessor** |
| equ \(alias: =\) | EQUate. Must be preceded by a label. The argument must be a numeric expression, the result is assigned to the label. I.e.**VIDRAM equ $4000** |
|  **Conditional blocks** |
| .if_cond_ | Contional assembly. The argument must be a numeric expression, a result of 0 is considered as false, any other as true. If the argument is true the following code is assembled until the end of the IF section is encountered, else is ignored. The IF section is ended with a ENDIF directive. IF can't be nested. |
| .ifn_cond_ | IF NOT |
| .endif | End of the IF block |
|  **Macros and blocks** |
| .macro_macro\_name_ | Defines a macro, see [the chapter about macros](/macros.md). |
| .rept_count_ | Repeat a block of code substituing arguments. See [the chapter about macros](/macros.md). |
| .endm | End of MACRO definition or REPT cycle. |
| .block | Start of logical block. All labels, defined in this block, are local. It means you can’t reference them from outside the block. If you want to define a label globally, simply prefix it with ‘@’, like @LABEL: Good idea is to enclose INCLUDEd code into block. |
| .endblock | End of BLOCK. |
| **65816 directives** |
| .m8 | Accumulator is 8bit |
| .m16 | 16bit accumulator |
| .x8 | index register is 8bit |
| .x16 | 16bit index |

##  {#macros}



