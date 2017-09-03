Barebone emulator, or “generic emulator”, is a customizable emulator of CPU + memory + serial terminal. You can configure it by simple config file. Let’s long story short…

First of all, prepare a config file, e.g. “mycomputer.emu”. Extension “.emu” is mandatory. File should contain this:
```
cpu Z80

memory.ram.from 0x1000
memory.ram.to 0x13ff
memory.rom.from 0x0000
memory.rom.to 0x0fff

;serial simple
;serial.in 1
;serial.out 1
;serial.status 0
;serial.status.available 0x20
;serial.status.ready 0x02

serial 6850
serial.data 0x81
serial.control 0x80

terminal.caps 1
```

Format is really simple: Each line contains one directive. Lines with “;” at the same beginning are comments. Here are parameters:

| cpu | can be “I8080”, “Z80” or “C6502” |
| :--- | :--- |
| memory.ram.from | starting address for RAM \(please follow the 0x… convention for hexadecimal numbers\) |
| memory.ram.to | last address of RAM |
| memory.rom.from, memory.rom.to | Same as above, but for ROM \(your source code should lays here\) |
| serial | Type of serial port. You can select “6850” or “simple”. See below |
| serial.data, serial.control | For 6850 serial – two ports. |
| serial.in, serial.out | in and out ports for “simple serial port” |
| serial.status | “Simple port” status. Read only port. It returns “serial.status.available” when there is any data for read, and “serial.status.ready” if port is ready to send a data. |
| terminal.caps | 1 sets terminal to caps lock \(default value 0 means “no caps”\) |
| serial.interrupt | value 1 means that terminal invoke CPU interrupt on keypress \(=serial data is ready for read\). Default 0 means “no interrupt”. |

Two serial ports are available – 6850 is the standard ACIA circuit, “simple” is a generic serial port with no complicated functions, just read and send bytes.

How to use it? It’s simple, just tell which emulator should use by the “.engine” directive \(without “.emu”\), like this:

```
org 0
.engine mycomputer

    di
    ld sp, 0x1400
    ld a, 15h
    out (80h),a
    ld hl, hello
    call printhl

endless:
    call geta
    jr z,endless
    inc a
    call printa
    
    jr endless
    
printhl: 
    ld a,(hl)
    or a
    ret z
    call printa
    inc hl
    jr printhl
    
printa: 
    push af
serdy:  
    in a,80h
    and 02
    jr z,serdy
    pop af
    out 81h,a
    ret
    
geta:
    in a,80h
    and 01h
    ret z
    in a,81h
    or a
    ret
    
hello:
    db "Hello World!",0x0d,0x0a,0
```


It’s a simple “hello” program, using the above configuration. Just save it as “test.z80” and click to “Emulate \(F10\)”. It should compile and run emulator with given configuration.

For 6502-based computer you can use this config file \(e.g.”my6502.emu”\):

```
cpu C6502

memory.ram.from 0x0000
memory.ram.to 0x0fff
memory.rom.from 0xf000
memory.rom.to 0xffff

serial 6850
serial.data 0xa001
serial.control 0xa000
serial.map 1
```

Please notice the “serial.map” directive. It means that serial port is not mapped into “I/O” space \(like Z80/8080 does\), but into memory space. Serial.data and serial.control are addresses now.

6502 requires RAM in a bottom part of address space, ROM at the top.

Try this code:

```
.engine my6502

ACIA         =  $A000 
ACIACONTROL  =  ACIA+0 
ACIASTATUS   =  ACIA+0 
ACIADATA     =  ACIA+1 

          .ORG    $FFFC 
          DW      reset 
          DW      reset
          
.org $f000

RESET:          
          LDX     #$FF 
          TXS     
          LDA     #$15
          STA     ACIAControl 
 
          LDY     #0 
LOOP:     
          LDA     Message,Y
          BEQ     KEY
          JSR     SEROUT
          INY
          BNE     LOOP
KEY:      LDA     ACIAStatus
          AND     #1
          BEQ     KEY
          LDA     ACIAData 
; simple data mangling
          SEC
          ADC #0
          JSR     serout 
 
          JMP     KEY
 
Message:   
          DB      $0C,"My hovercraft is full of eels!",$0D,$0A,$00 
 
SEROUT:   PHA
SO_WAIT:  LDA     ACIAStatus
          AND     #2
          BEQ     SO_WAIT
          PLA
          STA     ACIAData
          RTS
```

Other processors and peripherals are on its way…

