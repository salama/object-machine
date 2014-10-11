## Bootstrapping

Since the implementation of the machine is not physical, we will need to create an executable. This is a multi-step process that we will go into in the implementation chapter. The result will provide an excellent test as the machine will ultimately need to be able to recreate itself.

A basic understanding nevertheless helps to understand the design of the machine better.

#### Compiler bootstrap

Since a compiler is familiar to every programmar, let's start by looking at a how that bootstraps itself. A jiting virtual machine build on that, but makes the distinction of compile and runtime more complicated.

If CS is the compilers source code and CC a preexisting compiler for the language, then we call the executable that CC creates by compiling CS, CX:

```
CS --> CC --> CX
```

Since CS is the source for a compiler, CX is now a compiler. So if we use the compiler CX to compiler CS, we get CY.

```
CS --> CX --> CY
```

CX should be functionaly identical to CY, that is for every input given to CX and CY, they should produce functionally equivalent programs. Most languages may be implemented with small differences, so CX does not need to be identical to CY, but if we compile again we get a  compiler CZ that should indeed be identical to CY.

The process relies off course on a previous implementation of the language, CC in this case.

### Machine bootstrap

To use a similar bootstrap process as described, we need a language with an exising implementation. We choose ruby and call the existing machine rbx, but note that it does not matter which implementation we use.

To make things simpler we explain a simplified version and demonstrate how to create a language dependant machine. Chapter 4 will modify this for the language independant machine we design in chapter 2.

So if RS is the ruby source for our machine, we use rbx to create a machine RM

```
RS --> rbx --> RM
```

RM in turn can be used to create a machine RN which is functionaly equivalent and so on. While RM is not the machine OM that we design, it is a superset. We could say the RS is made up of the ruby dependent part RDS and a language independant part OS. We note that while we use rbx, OS is expressed in ruby, but that does not make it language dependant.

We will see the language independant format in chapter 4 and how that changes the very simplified process above.
