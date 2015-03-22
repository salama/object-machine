## Assembly

Assembly language is normally a text format of instruction memnonics with arguments that represent registers or numbers or switches. Assembling is then the parsing and conversion into a binary format. Since we have no external format for the Arm Instructions we call the process of creating binary encoded data from these instructions assembling.

The binary format is pure data, unlike the objects we have been using there is no functionality. When a binary gets loaded into memory, to be executed, it is loaded at an address that is possibly unknown at compile time. This presents a problem in two ways.

### Calling and branching

To call a function we need the address of the function, which we can not per se know. But we do know the position of the instruction *relative* to the function address. This relation is not changed by the loading and so we can use that to calculate the function address. This method is called pc (program counter) relative addressing and is the preferred method on cpu's that support it directly. Even on cpus that don't it can be implemented with a little maths.

Branches work basically the same, and this is the reason we have Blocks, as Blocks represent branch targets.

### References

An object reference is the address of an object, ideally in the loaded executable. But all we know is the address of objects in the binary file, possibly relative to a elf section start.

We could try to solve the problem in a similar relative way as above. Even the pc is not a useful reference point, the object adress of the object that contains the reference, is. But quit apart from the math overhead and extra compilation that creates for *every* object access, it affects even new objects.

Similar to a c program, our implemetation starts off with a small binary and when needing more space, creates more objects at runtime. References to these objects would also have to be relative, even though by now their real addresses are in fact known. Thus adding unneccessarily to the runtime burden.

The better solution is called relocation and basically fixes any address in the binary by adding the base address where the binary is loaded. When the binary is 0 relative this results in correct addresses. Relocation is part of the starndard c linking process, but as we do not (or rather minnimally) participate in the c conventions, we have to do this ourselves.

So instead of calling a main function at start, we call an __init__ that does the fixing. Our complete rtti makes this process not only possibe, but quite easy.

### Known addresses

For some cpu/os combinations the above problems can be circumvented. Some OS's load binaries at known constant addresses. Linux on arm is such a case.
