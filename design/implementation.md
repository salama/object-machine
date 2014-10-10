# Design and Implementation

Object orientatation has been around so long, today many people learn it as their first computer language. This is a good thing, as object orientation makes the task of programming a computer more understandable.
But while the concepts of object orientation are firmly established, implementations still elude most people as complicated, cryptic, almost a black art.

So the main goal is to clarify how an object oriented language is implemented. And since object orientation is the best way to descibe such an effort we will use that.
As we will see this is not the first time this has been attempted, but we will also see how previous attempts fall short of this goal.

And to prove that the design is workable, viable in a way, we will show how the design is mapped to current hardware. We will need a good object oriented abstraction of current hardware to do this. It should be noted however that while this validates the design on current hardware, the design could be implemented straight in silicon, maybe on an fpga.
