# Appendix

## Object file format

For completeness we add a brief description of the object file format used by the object machine.
Sof is not essential to the Machine, but was used in a previous version.

The format is not unlike yaml, but much more compact in that it allows several items on one line.
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
The written notation is a little like a constructor without the new.
For objects with less than 5 simple arguments:
```
    MemoryInstruction(:right => 1, :operand => 0, :pre_post_index => 1, :add_offset => 0, :position => 456)
```

For objects that use have objects as members, the attributes are written below the object header. The header will always include the class name, and possibly short attributes.
```
Virtual::Block(:position => 440, :length => -1, :name => :return)
 :codes -Arm::MoveInstruction(:operand => 0, :rn => :r0, :position => 440)
    :attributes {:opcode => :mov, :update_status => 0, :condition_code => :al}
    :to Register::RegisterReference(:symbol => :r0)
    :from Register::RegisterReference(:symbol => :r3)
```

### Classes derived from Hash or Array

Especially in the object machines runtime, some classes derive from Array. The notation for
any object of class that derives from Array or Hash is to append the above Notation for Hash or Array.

For example the Class Type, that derives from Array. As an Array it stores Symbols, so the instance that represents the Type of the Module is
```
Parfait::Type(:object_class => ->Module, :type => ->Type_Type) - :name
   - :instance_methods
   - :super_class
   - :meta_class
```

### References

An object file is a complete unit, but a tree representation of a graph.
References avoid duplication and endless recursion. The syntax for dereferencing is simply:

```
    ->25
```

and the reference is created by adding an
```
    &25
```

before any object, like
```
     :result &25 Register::RegisterReference(:symbol => :r4)
```

A class may name references to instances by defining
```
    def sof_reference_name
        return some_string
    end
```

The Sof Writer will still ensure that these references are globally unique and contain no spaces. But this feature makes for much more readable files as the example shows.

The Writer also ensures that the actual object is written at its most shallow occurrence in the tree, and any references to that object occur deeper in the tree.

### Example

Sof is meant to write object files. Below is an example of a minimal object space of the Parfait run-time. It is meant to illustrate the readability of a non-trivial object graph

```
&space Parfait::Space()
 :classes - :Value => &Value Parfait::Class(:name => :Value, :instance_methods => [])
   :object_type Parfait::Type(:object_class => ->Value, :type => ->Type_Type)
   :type &Class_Type Parfait::Type(:object_class => ->Class, :type => ->Type_Type) - :object_type
   - :name
   - :instance_methods
   - :super_class
   - :meta_class
 - :Integer => &Integer Parfait::Class(:name => :Integer, :instance_methods => [], :super_class => ->Value, :type => ->Class_Type)
   :object_type Parfait::Type(:object_class => ->Integer, :type => ->Type_Type)
 - :Kernel => &Kernel Parfait::Class(:name => :Kernel, :super_class => ->Value, :type => ->Class_Type)
   :instance_methods - &putstring Parfait::Method(:type => ->Method_Type, :for_class => ->Kernel, :name => :putstring, :arg_names => [], :locals => [], :tmps => [])
     :code Parfait::BinaryCode(:type => ->BinaryCode_Type, :name => :putstring)
     :info Virtual::CompiledMethodInfo(:return_type => Virtual::Unknown, :constants => [])
      :blocks - Virtual::Block(:name => :enter)
        :codes - Register::SaveReturn(:register => :r0, :index => 1)
        - Register::LoadConstant(:constant => ->space)
          :value &r4 Register::RegisterReference(:symbol => :r4)
        - Register::GetSlot(:reference => ->r4, :index => 5)
          :value &r5 Register::RegisterReference(:symbol => :r5)
        - Register::RegisterTransfer(:to => ->r5)
          :from Register::RegisterReference(:symbol => :r2)
        - Register::GetSlot(:value => ->r5, :reference => ->r5, :index => 2)
        - Register::SetSlot(:value => ->r5, :reference => ->r4, :index => 5)
      - Virtual::Block(:name => :return)
        :codes - Register::RegisterTransfer()
          :from Register::RegisterReference(:symbol => :r0)
          :to Register::RegisterReference(:symbol => :r3)
        - Register::GetSlot(:value => :r0, :reference => :r3, :index => 0)
        - Register::GetSlot(:value => :r1, :reference => :r0, :index => 3)
        - Register::GetSlot(:value => :r2, :reference => :r0, :index => 6)
        - Register::FunctionReturn(:register => :r0, :index => 1)
   - Parfait::Method(:type => ->Method_Type, :for_class => ->Kernel, :name => :__init__, :arg_names => [], :locals => [], :tmps => [])
     :code Parfait::BinaryCode(:type => ->BinaryCode_Type, :name => :__init__)
     :info Virtual::CompiledMethodInfo(:return_type => Virtual::Integer, :constants => [])
      :blocks - Virtual::Block(:name => :enter)
        :codes - Register::SaveReturn(:register => :r0, :index => 1)
        - Register::LoadConstant(:constant => ->space)
          :value &r4-35 Register::RegisterReference(:symbol => :r4)
        - Register::GetSlot(:reference => ->r4-35, :index => 5)
          :value &r5-36 Register::RegisterReference(:symbol => :r5)
        - Register::RegisterTransfer(:to => ->r5-36)
          :from Register::RegisterReference(:symbol => :r2)
        - Register::GetSlot(:value => ->r5-36, :reference => ->r5-36, :index => 2)
        - Register::SetSlot(:value => ->r5-36, :reference => ->r4-35, :index => 5)
        - Register::GetSlot(:index => 3)
          :value &r4-39 Register::RegisterReference(:symbol => :r4)
          :reference Register::RegisterReference(:symbol => :r1)
        - Register::SetSlot(:value => ->r4-39, :index => 3)
          :reference Register::RegisterReference(:symbol => :r1)
        - Register::RegisterTransfer()
          :from Register::RegisterReference(:symbol => :r3)
          :to Register::RegisterReference(:symbol => :r0)
        - Register::GetSlot(:value => :r1, :reference => :r0, :index => 3)
        - Register::FunctionCall(:method => ->main)
      - Virtual::Block(:name => :return)
        :codes - Register::RegisterTransfer()
          :from Register::RegisterReference(:symbol => :r0)
          :to Register::RegisterReference(:symbol => :r3)
        - Register::GetSlot(:value => :r0, :reference => :r3, :index => 0)
        - Register::GetSlot(:value => :r1, :reference => :r0, :index => 3)
        - Register::GetSlot(:value => :r2, :reference => :r0, :index => 6)
        - Register::FunctionReturn(:register => :r0, :index => 1)
   :object_type Parfait::Type(:object_class => ->Kernel, :type => ->Type_Type)
 - :Object => &Object Parfait::Class(:name => :Object, :super_class => ->Kernel, :type => ->Class_Type)
   :instance_methods - &main Parfait::Method(:type => ->Method_Type, :for_class => ->Object, :name => :main, :arg_names => [], :locals => [], :tmps => [])
     :code Parfait::BinaryCode(:type => ->BinaryCode_Type, :name => :main)
     :info Virtual::CompiledMethodInfo(:return_type => Virtual::Unknown, :constants => [:Hello])
      :blocks - Virtual::Block(:name => :enter)
        :codes - Register::SaveReturn(:register => :r0, :index => 1)
        - Register::LoadConstant(:constant => ->space)
          :value &r4-45 Register::RegisterReference(:symbol => :r4)
        - Register::GetSlot(:reference => ->r4-45, :index => 5)
          :value &r5-46 Register::RegisterReference(:symbol => :r5)
        - Register::RegisterTransfer(:to => ->r5-46)
          :from Register::RegisterReference(:symbol => :r2)
        - Register::GetSlot(:value => ->r5-46, :reference => ->r5-46, :index => 2)
        - Register::SetSlot(:value => ->r5-46, :reference => ->r4-45, :index => 5)
        - Register::LoadConstant(:constant => ->space)
          :value &r4-47 Register::RegisterReference(:symbol => :r4)
        - Register::GetSlot(:reference => ->r4-47, :index => 5)
          :value &r5-48 Register::RegisterReference(:symbol => :r5)
        - Register::RegisterTransfer(:to => ->r5-48)
          :from Register::RegisterReference(:symbol => :r2)
        - Register::GetSlot(:value => ->r5-48, :reference => ->r5-48, :index => 2)
        - Register::SetSlot(:value => ->r5-48, :reference => ->r4-47, :index => 4)
        - Register::GetSlot(:index => 3)
          :value &r4-49 Register::RegisterReference(:symbol => :r4)
          :reference Register::RegisterReference(:symbol => :r1)
        - Register::SetSlot(:value => ->r4-49, :index => 3)
          :reference Register::RegisterReference(:symbol => :r3)
        - Register::LoadConstant(:constant => :putstring)
          :value &r4-50 Register::RegisterReference(:symbol => :r4)
        - Register::SetSlot(:value => ->r4-50, :index => 4)
          :reference Register::RegisterReference(:symbol => :r3)
        - Register::LoadConstant(:constant => :Hello)
          :value &r4-53 Register::RegisterReference(:symbol => :r4)
        - Register::SetSlot(:value => ->r4-53, :index => 5)
          :reference Register::RegisterReference(:symbol => :r1)
        - Register::GetSlot(:index => 5)
          :value &r4-54 Register::RegisterReference(:symbol => :r4)
          :reference Register::RegisterReference(:symbol => :r1)
        - Register::SetSlot(:value => ->r4-54, :index => 0)
          :reference Register::RegisterReference(:symbol => :r3)
        - Register::RegisterTransfer()
          :from Register::RegisterReference(:symbol => :r3)
          :to Register::RegisterReference(:symbol => :r0)
        - Register::GetSlot(:value => :r1, :reference => :r0, :index => 3)
        - Register::FunctionCall(:method => ->putstring)
      - Virtual::Block(:name => :return)
        :codes - Register::RegisterTransfer()
          :from Register::RegisterReference(:symbol => :r0)
          :to Register::RegisterReference(:symbol => :r3)
        - Register::GetSlot(:value => :r0, :reference => :r3, :index => 0)
        - Register::GetSlot(:value => :r1, :reference => :r0, :index => 3)
        - Register::GetSlot(:value => :r2, :reference => :r0, :index => 6)
        - Register::FunctionReturn(:register => :r0, :index => 1)
   :object_type Parfait::Type(:object_class => ->Object, :type => ->Type_Type)
 - :Word => &Word Parfait::Class(:name => :Word, :instance_methods => [], :super_class => ->Object, :type => ->Class_Type)
   :object_type Parfait::Type(:object_class => ->Word, :type => ->Type_Type)
 - :List => &List Parfait::Class(:name => :List, :instance_methods => [], :super_class => ->Object, :type => ->Class_Type)
   :object_type Parfait::Type(:object_class => ->List, :type => ->Type_Type)
 - :Message => &Message Parfait::Class(:name => :Message, :instance_methods => [], :super_class => ->Object, :object_type => ->Message_Type, :type => ->Class_Type)
 - :MetaClass => &MetaClass Parfait::Class(:name => :MetaClass, :instance_methods => [], :super_class => ->Object, :type => ->Class_Type)
   :object_type Parfait::Type(:object_class => ->MetaClass, :type => ->Type_Type)
   :object_type Parfait::Type(:object_class => ->MetaClass, :type => ->Type_Type)
 - :BinaryCode => &BinaryCode Parfait::Class(:name => :BinaryCode, :instance_methods => [], :super_class => ->Word, :type => ->Class_Type)
   :object_type &BinaryCode_Type Parfait::Type(:object_class => ->BinaryCode, :type => ->Type_Type)
 - :Space => &Space Parfait::Class(:name => :Space, :instance_methods => [], :super_class => ->Object, :object_type => ->Space_Type, :type => ->Class_Type)
 - :Frame => &Frame Parfait::Class(:name => :Frame, :instance_methods => [], :super_class => ->Object, :object_type => ->Frame_Type, :type => ->Class_Type)
 - :Type => &Type Parfait::Class(:name => :Type, :instance_methods => [], :super_class => ->List, :object_type => ->Type_Type, :type => ->Class_Type)
 - :Class => &Class Parfait::Class(:name => :Class, :instance_methods => [], :super_class => ->Module, :object_type => ->Class_Type, :type => ->Class_Type)
 - :Dictionary => &Dictionary Parfait::Class(:name => :Dictionary, :instance_methods => [], :super_class => ->Object, :type => ->Class_Type)
   :object_type Parfait::Type(:object_class => ->Dictionary, :type => ->Type_Type) - :keys
   - :values
 - :Method => &Method Parfait::Class(:name => :Method, :instance_methods => [], :super_class => ->Object, :type => ->Class_Type)
   :object_type &Method_Type Parfait::Type(:object_class => ->Method, :type => ->Type_Type) - :name
   - :code
   - :arg_names
   - :locals
   - :tmps
 - :Module => &Module Parfait::Class(:name => :Module, :instance_methods => [], :super_class => ->Object, :type => ->Class_Type)
   :object_type Parfait::Type(:object_class => ->Module, :type => ->Type_Type) - :name
   - :instance_methods
   - :super_class
   - :meta_class
 :frames - ->1
 - Parfait::Frame(:type => ->Frame_Type)
 - Parfait::Frame(:type => ->Frame_Type)
 - Parfait::Frame(:type => ->Frame_Type)
 - Parfait::Frame(:type => ->Frame_Type)
 :messages - ->2
 - Parfait::Message(:type => ->Message_Type)
 - Parfait::Message(:type => ->Message_Type)
 - Parfait::Message(:type => ->Message_Type)
 - Parfait::Message(:type => ->Message_Type)
 :next_message &2 Parfait::Message()
  :type &Message_Type Parfait::Type(:object_class => ->Message, :type => ->Type_Type)
 :next_frame &1 Parfait::Frame()
  :type &Frame_Type Parfait::Type(:object_class => ->Frame, :type => ->Type_Type) - :locals
  - :tmps
 - Parfait::Message(:type => ->Message_Type)
 - Parfait::Message(:type => ->Message_Type)
 - Parfait::Message(:type => ->Message_Type)
 - Parfait::Message(:type => ->Message_Type)
 :next_message &2 Parfait::Message()
  :type &Message_Type Parfait::Type(:object_class => ->Message, :type => ->Type_Type)
 :next_frame &1 Parfait::Frame()
  :type &Frame_Type Parfait::Type(:object_class => ->Frame, :type => ->Type_Type) - :locals
  - :tmps
 :type &Space_Type Parfait::Type(:object_class => ->Space)
  :type &Type_Type Parfait::Type(:object_class => ->Type, :type => ->Type_Type) - :object_class - :classes
 - :frames
 - :messages
 - :next_message
 - :next_frame

```
