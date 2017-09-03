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

##  {#processor-specific-syntax}



