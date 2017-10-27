## Source code format. {#source-code-format}

Line can begin with a label. Label should be followed by ":", but colon can be omitted.

Everything after a ; in a line is a comment \(unless the ; is part of a string literal, of course\). There are no multiline comments.

String literals are written to the object file without any character set translation. In case you use punctuated character, the lower byte of its Unicode representation will be used.

Blanks are significative only in string literals and when they separate lexical elements. Any number of blanks has the same meaning as one. A blank between operators and operands is allowed but no required except when the same character has other meaning as prefix \('$' and '%', for example\).

### Literals

Numeric literals can be written in decimal, binary, octal and hexadecimal formats. Several formats are accepted to obtain compatibility with the source format of several assemblers.

* A literal that begins with $ is a hexadecimal constant, except if the literal is only the $ symbol.

* A literal that begins with % is a binary constant, except if the literal is only the % symbol, in that case, is an operator.

* A literal that begins with a decimal digit can be a decimal, binary, octal or hexadecimal. If the digit is 0 and the following character is an X, the number is hexadecimal. If not, the suffix of the literal is examined: D means decimal, B binary, H hexadecimal and O or Q octal, in any other case, is taken as a decimal. Take care, `FFFFh`, for example, is not a hexadecimal constant, is an identifier, to write it with the suffix notation you must do it as `0FFFFh`.

### String literals

There is one format of string literals. They should be double quote delimited. _Assembler can parse single quote form too, but it should produce an error, when delimitation is used in string, so please use double quotes._

A string literal of length 1 can be used as a numeric constant with the numeric value of the character contained. This allows expressions such as `'A' + 80h` to be evaluated as expected.

### Identifiers

Identifiers are the names used for labels, EQU symbols and macro names and parameters. The names of the CPU mnemonics, registers, and flag names, and of assembling directives are reserved and can not be used as names of identifiers. Reserved names are case insensitive, even if case sensitive mode is used.

Identifiers are not case sensitive. Internally are converted to uppercase.

### Expressions

Parser can evaluate simple math expressions, with all of the common operators, like +, -, /, \*, \# \(modulo\). You can use identifiers as a variables too, e.g. `LOOP + 3`.

There are some specials here, like string repetitions \(`"A"\*3` produces `"AAA"`\) or upper / lower part of identifier value. If LOOP is 0x1234, then &lt;LOOP means 0x34, &gt;LOOP means 0x12

### Listing

For each compiled file the listing \(.lst\) is generated. You can see all lines with their addresses and opcodes. At the bottom is all variables dump as well as cross-reference \(where is given variable defined and where is used\) 



