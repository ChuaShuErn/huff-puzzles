// what is mstore?
// why stack [0,5] after mstore gives 0\*31 + 5
// then why must we have 2 pushes before returning mstore?

//EVM contracts can only return values that have been stored within the current executions memory frame.
//This is as the return opcode takes two values as inputs.

The offset of memory to start returning from, and the length of memory to return.
In this case the return opcode will consume [0x00, 0x20].
Or 32 bytes in memory starting from byte 0.

This explains what 0x00 mstore is there for. mstore takes two items from the stack,
[location_in_memory, value].
In our case we have [0x00, 0x5],
this stores the value 5 into memory.

removing the last two pushes causes a stack underflow

This is because, we need to tell the evm, to

1. look at the stack
2. see that the stack has two arguments
3. 0x00 and 0x20
4. this tells the EVM to return 32 bytes from [0x00, 0x20] in memory

to return anything, the return value must be stored in memory?
GPT Explanation: Yes, The return opcode also takes two parameters: the first is the starting memory location of the data you wish to return, and the second is the size of the data in bytes.

// In solidity, the compiler will pad short strings such that it will fit into 32 bytes
// in huff, developer needs to manually pad short strings
