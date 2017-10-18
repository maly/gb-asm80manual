## Processor-specific syntax {#processor-specific-syntax}

### 6502

* **Zero page**

  Assembler tries to determine if zero page mode is suitable. It needs the operand value is computable in the first pass \(so no forward reference, no intensive math etc.\) If you need implicitly select zero page mode, simply prepend asterisk \(\*\) sign before the operand.

### 6809

* **Direct, or extended?**

  Assembler tries to determine which mode is suitable. It needs the operand value is computable in the first pass \(so no forward reference, no intensive math etc.\) If you need implicitly select one mode, use signs 
  &lt;
   \(for direct\) or 
  &gt;
   \(for extended\) right before the operand.

### 6800

* **LDA A or LDAA?**

  Use literally what you want to use. Compiler internally transfer all of these instructions into long syntax \(without a space\), so LDA A becomes LDAA etc.
  
### 8008

* **LAB or MOV A,B?**

  ASM80 supports both syntax flavors for Intel 8008, the older one (with mnemonics like LAB, LDA, LAD, CAL etc.) as well as the newer one, compatible with 8080 (MOV A,B, MVI etc.)
  
* **CPE, SBC etc. ambiguity**
  ASM80 performs an intelligent decision between old and new syntax. Unfortunately, some instruction in old syntax (like CPE - compare A and E) has a new meaning in new syntax (CPE is Call When Parity Even). ASM80 can decide on the base of parameters. "CPE" without parameters is "old Compare", "CPE $0123" is "new CALL".



## {#generic-emulators}



