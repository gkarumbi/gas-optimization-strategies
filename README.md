======================================================================================

Introduction
------------

Smart contracts are an essential part of the Ethereum network, and their performance is critical to the overall health of the ecosystem. 
One of the key factors affecting smart contract performance is the cost of executing code, which is measured in terms of gas. 
Gas is a unit of computation used in the Ethereum network, and it is required for every operation performed on the blockchain. 
To ensure that smart contracts are efficient and cost-effective, it's important to optimize their gas usage. 
In this article, we'll discuss some strategies for optimizing gas usage in Solidity.

- [ ] **Split Require and Revert Statements Longer Than 32 Bytes**
In Solidity, the require statement is used to enforce conditions in a smart contract. If the conditions are not met, the contract execution stops and the transaction is marked as failed. Similarly, the revert statement is used to undo the changes made to the contract and revert back to the previous state.

If your require or revert statement is longer than 32 bytes, you should split it into two or more statements to save gas. This is because longer statements consume more gas compared to shorter ones.

- [ ] **Use x = x + y Instead of x += y**
In Solidity, the += operator is a shorthand for x = x + y. However, the += operator is less efficient in terms of gas consumption compared to the x = x + y expression. So, instead of using x += y, you should use x = x + y to save gas.

- [ ] **Use unchecked in Loops**
In Solidity, the unchecked keyword can be used to suppress overflow and out-of-bounds checks. When using a loop, if you know that the index variable will not overflow or go out of bounds, you can use unchecked to save gas.

For example, consider the following two contracts:

scss
Copy code
pragma solidity ^0.8.0;
contract Test1 {
    function loop() public pure {
        for(uint256 i = 0; i < 100; i++) {
        }
    }
}

pragma solidity ^0.8.0;
contract Test2 {
    function loop() public pure {
        for(uint256 i = 0; i < 100;) {
            unchecked {
                i++;
            }
        }
    }
}
The loop() function in Test1 costs more than 31K gas, whereas the loop() function in Test2 costs only 25.5K gas.

- [ ] **Inline Internal Functions Called Once**
In Solidity, internal functions are functions that can only be called from within the contract. If you have an internal function that is called only once, you can inline it to save gas. This is because inlining eliminates the overhead of calling a separate function, thus reducing the gas consumption.

- [ ] **Using *private* rather than public for *constants* saves gas - *Ex. uint256 public constant BIPS_ONE = 1e4;***

- [ ] **Use `calldata` instead of `memory` for function parameters  -** 
Mark data types as `calldata` instead of `memory` where possible. This makes it so that the data is not automatically loaded into memory. If the data passed into the function does not need to be changed (like updating values in an array), it can be passed in as `calldata`

Ex 1: 

```
contract C {
    function add(uint[] memory arr) external returns (uint sum) {
        uint length = arr.length;
        for (uint i = 0; i < arr.length; i++) {
            sum += arr[i];
        }
    }
}
```

```
contract C {
    function add(uint[] calldata arr) external returns (uint sum) {
        uint length = arr.length;
        for (uint i = 0; i < arr.length; i++) {
            sum += arr[i];
        }
    }
}
```

NB// The values in the array are not being changes they are just being used to return a sum

Ex. 2:

```solidity
function auctionID(Auction memory auction) external pure returns (uint256);
```

```solidity
function auctionID(Auction calldata auction) external pure returns (uint256);
```

NB// This code does not change the function parameters rather it changes something
