#### Class and methods

The top level declarations in a file may only be class definitions

    class Dictionary < Object
      int add(Object o)
        ... statements
      end
    end

The class hierarchy is explained in [the chapter on Parfait](./parfait.html). The superclass
may be left out and Object will be assumed.

Methods must be typed, both arguments and return. Generally class names serve as types, but int can
be used as a shortcut for Integer.

Code may not be outside method definitions, like may in ruby. A compiled program starts at the builtin
method __init__, that does the initial setup, an then jumps to Object.main

Classes are represented by class objects and methods my Method objects, so all information is available
at runtime.

#### Expressions

Soml distinguishes between expressions and statements. Expressions have value, statements perform an
action. Both are compiled to Register level instructions for the current method. Generally speaking
expressions store their value in a register and statements store those values elsewhere, possibly
after operating on them.

**Basic expressions** are numbers (integer or float), strings or names, either variable, argument,
field or class names. (normal details applicable). Special names include self (the current
receiver), and message (the currently executed method frame). These all resolve to a register
with contents.

      23
      "hi there"
      argument_name
      Object

A **field access** resolves to the fields value at the time. Fields must be defined by
field definitions, and are basically instance variables, but not hidden (see below).
The example below shows how to define local variables at the same time. Notice chaining, both for
field access and call, is not allowed.

      self.type
      l.object_class

A **Call expression** is a method call that resolves to the methods return value. If no receiver is
specified, self (the current receiver) is used. The receiver may be any of the basic expressions
above, so also class instances. The receiver type is known at compile time, as are all argument
types, so the class of the receiver is searched for a matching method. Many methods of the same
name may exist, but to issue a call, an exact match for the arguments must be found.

      Class c = self.get_class()
      c.get_super_class()

An **operator expression** is a binary expression, with either of the other expressions as left
and right operand, and an operator symbol between them. Operand types must be integer.
The symbols allowed are normal arithmetic and logical operations.

       a + b
       counter | 255
       mask >> shift

Operator expressions may be used in assignments and conditions, but not in calls, where the result
would have to be assigned beforehand. This is one of those cases where soml's low level approach
shines through, as soml has no auto-generated temporary variables.

#### Statements

We have seen the top level statements above. In methods the most interesting statements relate to
flow control and specifically how conditionals are expressed. This differs somewhat from other
languages, in that the condition is expressed explicitly (not implicitly like in c or ruby).
This lets the programmer express more precisely what is tested, and also opens an extensible
framework for more tests than available in other languages. Specifically overflow may be tested in
soml, without dropping down to assembler.

And **if statement** is started with the keyword if_ and then contains the branch type. The branch
type may be plus, minus, zero, nonzero or overflow. The condition must be in brackets and be any
expression. If may be continued with en else, but doesn't have to be, and is ended with end

      if_zero(a - 5)
        ....
      else
        ....
      end

A **while statement** is very much like an if, with off course the normal loop semantics, and
without the possible else.

      while_plus( counter )
        ....
      end

A **return statement** returns a value from the current functions. There are no void functions.

    return 5


A **field definition** is to declare an instance variable on an object. It starts with the keyword
field, must be in class (not method) scope and may not be assigned to.

    class Class < Object
      field List instance_methods
      field Type instance_type
      field Word name
      ...
    end

A **local variable definition** declares and possibly assign to a local variable. Local variables
are store in frame objects and the are last in search order. When resolving a name, the compiler
checks argument names first, and then local variables.

    int counter = 0

Any of the expression may be assigned to the variable at the time of definition. After a variable is
defined it may be assigned to with an **assignemnt statement** any number of times. The assignment
is like an assignment during definition, without the leading type.

      counter = 0

Any of the expressions, basic, call, operator, field access, may be assigned.

### Code generation and scope

Compiling generates two results simultaneously. The more obvious code for a function, but also an
object structure of classes etc that capture the declarations. To code part is explained in the
chapter on the register machine, and the object structure is explained in the chapter on Parfait.

The register machine abstraction is very simple, and so is the code generation, in favour of a simple
model. Especially in the area of register assignment, there is no magic and only a few simple rules.

The main one of those concerns main memory access ordering and states that object memory must
be consistent at the end of the statement. Since there is only only object memory in soml, this
concerns all assignments, since all variables are either named or indexed members of objects.
Also local variables are just members of the frame.

This obviously does leave room for optimisation as preliminary benchmarks show. But benchmarks also
show that it is not such a bit issue and much more benefit can be achieved by inlining.
