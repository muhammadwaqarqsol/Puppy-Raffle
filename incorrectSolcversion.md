# Findings

## Incorrect Solc Version

### Description
The contract `PuppyRaffle.sol` is compiled with an outdated version of Solidity, which may lack the latest security checks and features.

### Recommendation
It is advisable to update the Solidity compiler to a recent version to benefit from the
latest security checks and features. Consider deploying with any of the following Solidity versions: 0.8.18. 
Additionally, simplify the pragma statement to allow any of these versions.

### References
The recommendations take into account:
- Risks related to recent releases
- Risks of complex code generation changes
- Risks of new language features
- Risks of known bugs

### Action Taken
The Solidity compiler version has been updated to address this issue.

