# Appendix

## Object file format

For completeness we add a brief description of the object file format used by the object machine. 
This is what makes the machine language independent.

The format is not unlike yaml, but much more compact in that it adds it allows several items on one line. 
Also it is specific to the object machine, and as such uses class names un-scoped.

We will define this by example and refer to the implementation for details:

### Simple types
String
```
    "string"
```
Symbol
```
    :sym
```

Number
```
    123444
```

true , false and nil are written without quotes, lower case.

### Arrays

In compact form, ie 5 or less simple items
```
    ["string", 12344, .. ]
```
larger arrays or arrays with complex structures

```
    - "string"
    - [:another , "array" , :but, "small"]
    - .... (maybe hash or object)
```

### Hash

In compact form, ie 5 or less simple items
```
    {:key => "value" , :other => 12344, .. }
```
larger arrays or arrays with complex structures

```
    - :key => "value"
    - {}
    - ....
```

### Object

Objects can bee seen as a hash, but must have a class.
The notation is a little like a constructor without the new.
For objects with less than 5 simple arguments:
```
    MemoryInstruction(:right => 1, :operand => 0, :pre_post_index => 1, :add_offset => 0, :position => 456)
```

Or object that use have objects as members.
```
Virtual::Block(:position => 440, :length => -1, :name => :return)
           :codes -Arm::MoveInstruction(:operand => 0, :rn => :r0, :position => 440)
              :attributes {:opcode => :mov, :update_status => 0, :condition_code => :al}
              :to Register::RegisterReference(:symbol => :r0)
              :from Register::RegisterReference(:symbol => :r3)
```

### References

An object file is a complete unit, but a tree representation of a graph. 
References avoid duplication and endless recursion. The syntax for de-referencing is simply:

```
    *25
```

and the reference is created by adding an
```
    &25
```

before any object, like
```
     :result &25 Register::RegisterReference(:symbol => :r4)
```
