## What and how

This chapter defines our actual design goal and also principles we want to adhere to while designing.

Our primary goal is to design a language independent native object machine capable of running object oriented languages.

To do this we will have to define what makes a machine and what characterises an object oriented language. We will see how similar things were done previously, and how to "build" the initial version of such a mchine on current hardware.

### Design principles

But before we start with designing the actual machine, let's define guiding principles. Principles that will guide the decisions we have to make on the way.

#### Simple

This is off course difficult to define, so let's start with what it is not. Simplicity does not mean lack of complexity. Think of a butterfly, very simple , yet very complex.
Simplicity is rather the lack of complication.

#### Minimal

This is almost implied in simplicity, because when a design is minimal it usually is simple. Still, it is a seperate goal, that can be expressed succinctly by the mikrokernel idea:

If you can leave it out, do.

#### Extensible

Since we want to design a minimal system, it needs to be extensible easily. And as it is an open system, easily means a mechnism by which anyone can define more functionality for the machine. Off course with open classes wóne can always patch, but this is not what we mean. rather a defined interface for at least the central parts of type system and instruction set.

This also encourages external developers to improve the machine for different hardware or use cases. While obvious examples include cpu/os combinations, also gpu or mmu support should be possible.
