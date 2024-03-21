# Findings

## [I-4] Does not follow CEI, which is not a best practice

### Description
The `selectWinner` function in `PuppyRaffle.sol` does not follow the Checks-Effects-Interactions (CEI) pattern, which is considered a best practice for writing secure and understandable smart contracts.

```diff
(bool success,) = winner.call{value: prizePool}("");
require(success, "PuppyRaffle: Failed to send prize pool to winner");
_safeMint(winner, tokenId);
(bool success,) = winner.call{value: prizePool}("");
require(success, "PuppyRaffle: Failed to send prize pool to winner");
```
### Recommendation
It's best practice to adhere to the CEI pattern in smart contract development to improve code clarity and security. Refactor the selectWinner function to follow the CEI pattern by separating checks, effects, and interactions.

// Pseudocode for following CEI pattern
address winner = _selectRandomWinner();
require(winner != address(0), "PuppyRaffle: Winner cannot be zero address");

_transferPrize(winner, prizePool);
_mintToken(winner, tokenId);

### Action Taken
The selectWinner function has been refactored to follow the CEI pattern for improved code clarity and security.
