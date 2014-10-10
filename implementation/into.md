# Implementation

Both Object and Register machines are abstract, or virtual. To create an actual machine we need to create an executable.

This we do by compiling the code from the register machine and combining the code and binary representations of the objects in one file. Out test machine is running linux as the operating systems which uses elf as it's binary format. As we don't want to get too dependent, the elf encoding is kept very minimal.

The proces of compiling from the register machine instruction set to the arm instruction set usses the same idea as was used to compile to the register machine, ie passes. One Pass for each register instruction is registered and does a more or less one to one mapping to the arm names.