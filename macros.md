## Macros {#macros}

Define macro with .macro and .endm. You can use a parametric macros - any parameter is addressable by %%1, %%2, %%3, ... For example, such code:

```
   .macro decadd
      adi %%1
      daa
   .endm
   ; Use this macro
    decadd $22
```

will generate this:

```
   0000 ; Use this macro
    **MACRO UNROLL - DECADD
   0000 87 22 ADI $22
   0001 27    DAA
```

### Local labels

Macros has no local label mechanism. So if you define a label in a macro, it unrolls to the same label, ending with a "redefine label" error message.

There is a workaround to define unique label for each macro unrolling:

```
.macro xyz
loop%%M: inc a
   dec b
   jr nz,loop%%M
.endm
```

Now it is safe to use this macro repeatedly, because special placeholder "%%M" is replaced by string "M_"+line number. It provides a good enough workaround for local labels.

### Compound parameters

Let's imagine a macro:

```
.macro test
 db %%1
 dw %%2
.endm
```

If you use this macro in such form, everything is OK:
```
 test $12, $3456
```

Two parameters is OK. But what if you need the first DB is something like `db $de,$ad,$be,$ef`? You can use the compound parameter:
```
 test {$de,$ad,$be,$ef}, $3456
```

##  {#processor-specific-syntax}



