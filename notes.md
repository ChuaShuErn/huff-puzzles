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

For a function that accepts a single uint256 argument (which is 32 bytes), the calldata layout would look like this:

First 32-Byte Word:

The first 4 bytes contain the function selector.
The next 28 bytes contain the beginning of the uint256 argument.
Second 32-Byte Word:

The first 4 bytes of this word contain the remaining part of the uint256 argument.
The rest of this second word would be zeros if there are no additional arguments.

First 32-byte word: [Function Selector (4 bytes)] [First 28 bytes of uint256 Argument]
Second 32-byte word: [Last 4 bytes of uint256 Argument] [Padding (if no more arguments)]

JUMPI:
Any Non Zero value will cause it to jump
