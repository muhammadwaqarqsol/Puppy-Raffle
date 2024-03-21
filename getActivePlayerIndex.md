# Findings

## [L-1] `PuppyRaffle::getActivePlayerIndex` returns 0 for non-existent players and players at index 0 causing players to incorrectly think they have not entered the raffle

### Description
If a player exists in the `players` array at index 0, the `getActivePlayerIndex` function in `PuppyRaffle.sol` will return 0. However, according to the function's documentation, it should also return 0 if the player is not in the array. This discrepancy may lead to players incorrectly believing they have not entered the raffle when they have, causing confusion and potential gas wastage.

```solidity
function getActivePlayerIndex(address player) external view returns (uint256) {
    for (uint256 i = 0; i < players.length; i++) {
        if (players[i] == player) {
            return i;
        }
    }
    return 0;
}
```

### Impact
A player at index 0 may incorrectly think they have not entered the raffle and attempt to enter the raffle again, wasting gas.

### Proof of Concept
- User enters the raffle, becoming the first entrant.
- PuppyRaffle::getActivePlayerIndex returns 0.
- User misinterprets the result, assuming they have not entered the raffle correctly.

### Recommendations
- Revert the function call if the player is not found in the array instead of returning 0.
Alternatively, modify the function to return an int256 where -1 is returned if the player is not active.
