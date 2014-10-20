## Instruction Set

This chapter will define the instructions of the Register Machine. As we have no second cpu implementation, the main benefit of this is to define meaningfull names for the actions/instructions of the abstract machine so we can better understand what it does.

As this is not the focus of the book, we will keep it short and refer to the actual code for more details. Also the next chapter will give a very brief overview of what passes are implemented, and how those implement the object machine.

### GetSlot

While we have one instruction to move data in the object machine (Set), we need three in the Register Machine, to from and between registers.

GetSlot moves what is essentially an instance variable to a register. Often the register will be one of the scratch ones, but to implement the calling logic, the first three must be set.

GetSlot uses an object reference and an index, but the reference must be given as a register name. It moves the data at that index to the register given as first parameter.

## SetSlot

SetSlot is in essence the reverse of GetSlot, it stores data, given as a register name, at the offset into the referred object.

We will see below, that a Get/SetSlot combination  is **not** an implementation of Set, as type information needs to be updated.

### RegisterTransfer

RegisterTransfer transfers data from one register to another. This is used in the calling setup and message/frame generation.

### SaveReturn

SaveReturn is very much like a SetSlot, but the data is implicit, namely the return address. The instruciton abstracts the place where the actual machine stores the address. The address is stored in the register/offset combination given.

### FunctionCall

Transfers control to the given Method. At register level this does not do any setup or other, so for the Object Machine FunctionCall implementation regsiter shuffling has to happen.

### RegisterMain

This instruciton sets up the machine and starts execution at the method given. This is seperate from the FunctionCall to allow for different implementations of both the set and the initial control transfer.

### LoadConstant

Load the given Constant into the given register. Constants may be object references or integers. 

### FunctionReturn

Return from a Function. The return address is given as an offset to a base register address, as a reverse to the SaveReturn.
