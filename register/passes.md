# Register Passes

As we explained above, Passes are small chunks of code that each perform part of the compilation or transformation from one machine model to another.

Here we list the passes that are needed to implement the Object Machine in terms of the Register Machine. It should be stressed that this is a *minimal* set, and external software may both add additional passes, to implement additional instructions, and add passes to add to or change existing functionality.

As the project is not finalized, the list below is still growing.

The Naming convention we use is YYImplementation where YY is the Object Machine instruction that is being implemented.

## CallImplementation

Defines the method call, ie

- move the new_message to message
- unroll self from the message (move to right registers)
- nil the frame
- issue a register FunctionCall

## ReturnImplementation

Return from a method call by

- move message back to new_message
- restore the calling message
- restore self and frame from the message
- issue a FunctionReturn

## SetImplementation

Set moves data from one object to another in one oo instruction, but at the register level it is three steps:

- move data to temporary register with GetSlot
- move data to destination with SetSlot
- transfer the type information to the new Slot

