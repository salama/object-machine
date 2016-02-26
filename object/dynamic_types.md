## Dynamic Types

This chapter explains how we will use the static language described in the next chapter, to build a
dynamic system. As such it may be best to read the chapter on Sonl first.

By type we mean the derived classes from Value, so mainly Object and Integer and Float, not the
class. This differentiation comes from how the class can be determined once the type is known,
but not without that knowledge.

A similar mechanism can be used to expand the idea below to classes, but for now we deal in types
only.

### Types are known

The basic idea deviates from a traditional vm approach, so it needs a little explaining.
The normal way is to carry runtime  information al the time and check the type for a given operation.
This approach does not assume any knowledge of types and so needs to check all the time at runtime.

Our approach is that types are known all the time. But they may change. Even in a dynamically typed
language, types are known implicitly during compile time. And thus also when the program starts.

It is then "just" a matter of tracking changes. How this is done and what it requires is described
in the rest of the chapter. But it is important to understand the difference. We never assume that
a type is unknown, rather we start at a known state and track changes.

### Known start

The runtime is written in soml and as such completely typed.
When compiling ruby all types are known by the compiler at program start.
The values that are not assigned a known type, and thus have no start value, may either be deduced
through later assignments, or assigned a the object type.

All type information is recorded in the Types of the objects created at compile time. It helps
to remember that there is no information outside objects, also at runtime.

So when the program starts, it does so in a known state.

### Points of change

There is, maybe surprisingly, only one place where a type may change during runtime in soml. This is
the assignment. In a dynamic language it must be possible to assign different types to the same variable
or instance variable. Notice though that these two are the same thing in a purely object oriented
system, as all variables are either named or unnamed members of some object.

The assignment case actually has two sub cases, the static and dynamic one. For many cases the
assignment happens statically, for example programmers use nil to signal a not initialized variable
that is then assigned an integer. Such static cases can be dealt with at compile time relatively
easily. We'll describe how below, but basically the compiler generates code for this case.

With normal return semantics this would actually be the only way to change type. But it would be
rather restrictive or cumbersome, because if methods can only return one type, either an object must
be used to encode different possibilities or the calling code must "test" and then call different
functions or the same function in different branches to receive the differently typed results.
To avoid this, soml supports the concept of return different types, and to handle this correctly in
the caller this means different return addresses for different type.

In effect this makes every soml call more like an if statement, rather than the sequential statement
it is in other languages. The soml compiler generates error code for cases when soml does not
make precaution for a possible type change. But the ruby compiler generates soml code to check all
possibilities, so that the only place of error may come from not finding a correct method (see below).

### Type combinations

As objects come in small known sizes, and the amount of types is very limited, the total number
of possible combinations is not only limited, but relatively small.
In earlier versions the type information was encoded as integers, but since it is quite a small
number objects are used now. Maybe some thousands of objects,
10-30k memory, as a constant factor, independent of the size of system.

The compiler pre generates all combination for 1 and 2 lines ahead of time, plus off course all
used combinations for larger objects. The describing objects form a graph in which it is extremly
efficient to move one step. So if, for example, the third slot in an object changes from
object to integer, a single association in the graph has to be followed to find the appropriate
descriptor.

The change that the compiler generates is the same for the static and dynamic case. The type
descriptor of the object is changed. Not the Type, or class, just the type descriptor.

### Method signatures

Now that we have changing types for objects we must address the question of method calling.
Since Soml is static, the system must devise a way of knowing which calling the right method
for any given type combination.

From the previous, we know that the type signature is known, just not statically. Let's take a
simple example, a ruby function plus that adds two arguments and returns the result. Two
arguments means four combinations of types, let's start with the two integer case. Even there we can
see that a correct implementation may return an int or a BigInteger because it must check for the
overflow. The overflow case may convert the first arg and then call "itself". The other three cases
involve various forms of delegation to BigInteger. The main point being that we really need four
implementations for the one ruby function.

At the soml level this example also involves four methods, integer's and BigInteger's add with either
integer or BigInteger. Knowing the problem it is relatively easy to see how to do the implementation
of the ruby method in soml by checking all the cases. But off course we need the compiler to generate
the solution and so need a "formula".

Our formula is quite simple: we compile the same method for every type combination. So when faced
with compiling the example method plus, with it's two arguments (of unknown type), we go through the
possible combinations of types and compile the method for each. We end up with one method, as we
distinguish methods by name, but four implementations. We store those four implementations in a
associative structure with type combination to implementation in the method.

Given this formula, we need the add methods on Integer and BigInteger to also have all combinations.
As everything resolves to Parfait calls, this means we need Parfait to have method implementations
for all combinations. In fact Parfait does not have this, but the compiler generates exception code
for non-existing combinations when it first encounters a new method name. If the method is then
implemented for another combination later, that will overwrite the generated code. If not, these
generated functions will raise a run-time error.
