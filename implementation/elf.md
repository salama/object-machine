## Executable

During assembly, binary versions of the instructions are created. Blocks contain a list of
Instructions and thus get converted to a stream of binary. Methods in turn have a list of
blocks and so the whole Method becomes a binary stream.

Strings are other simple objects that similarly transform easily to their binary representation,
assuming a utf8 encoding.

Other objects contain a mix of integer and reference values that are binary encoded as well.
The memory layout described above demands type encoding for all values and a layout reference
for evey object.

For references and calls we need positions for all objects which are determined before assembly
starts. Objects are laid out in the order they are stored in the ObjectSpace, but all Method
objects are laid first.

### Elf

We use elf in a very minimal way, to get Linux to load the executable. Elf has a header, which we
produce suitably, and sections of which  the text section contains the code.

To help debugging, method names and object classes are exported. Currently the final result still
has to be run with ld -N.

