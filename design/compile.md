# Interpret and Compile

For many people object oriented languages are synonymous with slow. This is especially strange since even early Smalltalk implementations had native gui's. But mostly it is a misconception fueled by two facts:

- many object oriented language implementations choose to interpret
- object oriented languages are dynamic and that means more work at runtime

Let's look at both of those reasons and see how they relate to our object machine.

### Interpretation

There are several reaons to choose interpretation over compilation, and which is the most important of those will depend on the person or organisation making the choice.

Readily available compilation tools for antoher language but the one implemented is probably the primary factor for most implementors. Platform independence is highly rated and perceived as a very difficult task. If not difficult at least an unneccesary extra. Especially c has with gcc, and nowadays llvm, a solid multiplatform implementation. Also c is know to many so getting started may be quick. LLvm even has a vm toolkit, and even without implements an embeddable jit, an implementors dream come true.

For the object machine we care mostly about the arm cpu, and the fact that it should be possible to create a port with realtive ease. That is if a person knows the language (ruby) and the cpu (say intel) it should be possible to port the machine in less than a quarter of the time it took to create it. C has not started out the standardised, readily available platform that it is now, so why should an object oriented platform be different.

Especially when choosing another language to implement the target language interpretation is the straightforward choice. Implementing an object oriented language is difficult enough, without trying to manipulate the implementation at runtime. Also the tool mismatch of implementation and target language is hard to overcome.

Another reason to choose the interpretation would be to achieve some kind of platform independence. This may be the case for java, thus encouraring independant implementations. For an individual person the extra effort may not be worth it.
Nevertheless other languages have ad-hoc, or implicitly, defined instruction sets that serve as a basis for the interpretation.

In summary, while interpretation leads to an easy start and portable code, it neccessarily creates run-time overhead that can not be changed (or never has been). It also leads to a black box from the target language programmar.

### Compilation

The most well known example of an object oriented language that is compiled to executables is c++. C++, or c with classes as it was called in the beginning, started out by compiling to c. Later librabies were created and direct compilation to machine code implemented. The approach would probably still work, not least because c++ is not very dynamic.

C++ has defined object layouts with compile time known offsets into tables. With this is can only implement single inheritance, by layering the tables on each other and using offset. And by avoiding dynamic behaviour like this it manages to be only about 20-30 percent slower than c on common shootouts.

Another less well known, and much more recent, example is the crystal language (crystal-lang). Crystal borrows rubies ideas and syntax heavily, but adds some type syntax. It uses type inference to determine all types at run-time and flags any code where this can not be done as erroneous. Crystal, by now implemented in crystal, uses llvm to create executables similarly to c++.

As far as we know full compilation of a dynamic object oriented language has never been achieved.

## Dynamic vs. static

Object oriented languages are dynamic to a larger or smaller extent. C++ is probably there at the end of less dynamic while Smalltalk and ruby are as dynamic as possible. And when looking at the performance of these languages, even trying to calculate the compilation vs interpretation out of the equation, one is left with the fact that more dynamic means slower.
This is understandable as dynamic at runtime means work at runtime and work takes time.

At the moment we have to say that *theortically* one could optimze the dynamic code much more. One could *imagine* that if the same amount of time and expertise were spent on that, it would result in much improved performance.

Especially if the implementation language would be the target language and we wouldn't have the mismatch, all optimisations would just be more target language code. The emergence of llvm proves this as a valid direction. By using an object oriented language (llvm is written in c++), it made the task more aunderstandable. Alos gcc had grown over the years and decades and the internal languages used to ease the tasks that c makes hard were difficult to understand.

We hope to prove this theory, that a same language implementation will result in speed improvements.

