# Findings

## Floating Pragma

### Description
Using a floating pragma version in Solidity, as observed in `src/PuppyRaffle.sol` at line 3,
can introduce uncertainty and potential issues with future compiler updates.

### Recommendation
To ensure stability and predictability, it's recommended to use a specific version of Solidity in 
contracts rather than a floating pragma. For example, replace `pragma solidity ^0.7.6;` with `pragma solidity 0.7.6;`.

### Action Taken
The floating pragma has been replaced with a specific version pragma in the codebase.

