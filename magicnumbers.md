# Findings

## [I-5] Use of "magic" numbers is discouraged

### Description
The use of "magic" numbers, or number literals, in the `selectWinner` function of `PuppyRaffle.sol` can make the codebase less readable and harder to maintain. It is best practice to give these numbers a descriptive name to improve code readability.

Examples:
```solidity
uint256 public constant PRIZE_POOL_PERCENTAGE = 80;
uint256 public constant FEE_PERCENTAGE = 20;
uint256 public constant POOL_PRECISION = 100;

uint256 prizePool = (totalAmountCollected * PRIZE_POOL_PERCENTAGE) / POOL_PRECISION;
uint256 fee = (totalAmountCollected * FEE_PERCENTAGE) / POOL_PRECISION;
```

### Action Taken
The use of "magic" numbers has been replaced with descriptive constants for improved code readability and maintainability.
