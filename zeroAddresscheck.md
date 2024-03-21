# Findings

## Zero Address Check

### Description
The contract's constructor in `PuppyRaffle.sol` assigns values to address state variables without checking for the zero address (`address(0)`), which could lead to unexpected behavior.

### Instances:

- `PuppyRaffle::feeAddress` assignment in constructor at line 69
- `PuppyRaffle::previousWinner` assignment at line 159
- `PuppyRaffle::feeAddress` reassignment at line 182

### Recommendation
Implement checks for the zero address (`address(0)`) before assigning values to address state variables to prevent potential issues.

### Action Taken
Checks for the zero address have been implemented before assigning values to address state variables in the contract's constructor.

