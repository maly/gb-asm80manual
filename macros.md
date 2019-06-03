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

### Rich syntax (v2.5.2+)

A macro can be defined by standard syntax as `.macro name`, or more consistent way as `name: .macro` or `name .macro`.

### Formal parameters (v2.5.2+)

Now you can name formal parameters too. For example - assume a macro cpymem for copy memory content. Such macro has three arguments - source, destination and length. The old form of macro has _mute parameters_, just referenced by its number:

```
.macro cpymem
LD HL,%%1
LD DE,%%2
LD BC,%%3
LDIR
.endm
```

New formal parameters, introduced in revision 2.5.2, allows to write this:

```
.macro cpymem, src, dst, len
LD HL,src
LD DE,dst
LD BC,len
LDIR
.endm
```

Preprocessor also check if the number of given parameters is sufficient for the macro, i.e. you cannot specify less parameters than formal, like `cpymem 100, 200`.

All those named parameters are strictly local (in fact, they are replaced with its values before assembly phase).

##  {#processor-specific-syntax}



