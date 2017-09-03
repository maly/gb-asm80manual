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

## {#generic-emulators}



