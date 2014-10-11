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

#### Complete

This is just here for completeness, as it is almost implied in above statement.

Completeness means we achieve the set goal, which we will define in the next chapter. At a high level one could compare the machine to a real one and say it is mostly about creating capabilities. By no means do we aim to create any full language implementation, only the basis upon which a language can be implemented in an object oriented manner. The object machine should be to object oriented languages what a physical machine is to c.

