Built-in variables:

- `$@`: The full target filename. The file that needs to be built, such as a `.o` file being compiled from a .c file or a program made by linking `.o` files.
- `$*`: The target file with the suffix cut off. So if the target is `prog.o`, `$*` is `prog`, and `$*.c` would become `prog.c`.
- `$<`: The name of the file that caused this target to get triggered and made. If we are making `prog.o`, it is probably because `prog.c` has recently been modified, so `$<` is `prog.c`.

The rules:

Segments of the makefile have the form:

```make
target: dependencies
    script
```