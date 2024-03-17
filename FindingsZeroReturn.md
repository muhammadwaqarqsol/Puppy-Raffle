## [S-001] Misleading Return Value in `getActivePlayerIndex` Function (Root: User Misinterpretation + Impact: Confusion)

### Description:
The `getActivePlayerIndex` function in the `PuppyRaffle` contract is designed to return the index of a player in the `players` array. However, the function has a potential issue where if a player's address is at index zero, the function incorrectly returns zero, leading to user misinterpretation.

### Impact:
This issue can cause confusion for users, as they might incorrectly interpret a return value of zero to mean that the player is not found in the array, even if the player's address is present at index zero. Consequently, users may make incorrect assumptions about the status of their participation in the raffle.

### Proof of Concepts:
To demonstrate the issue, consider a scenario where a player's address is present at index zero of the `players` array. Calling the `getActivePlayerIndex` function with this player's address would return zero, potentially leading the user to believe that their address is not included in the array.

### Recommended Mitigation:
To mitigate this issue and improve clarity, it is recommended to adjust the return value when the player is not found in the array. Instead of returning zero, which could be misleading, the function should return a value that clearly indicates the absence of the player, such as `players.length`. By doing so, users will have a clearer understanding that their address is not present in the array when receiving a non-zero index value.

### Example Fix:
```solidity
function getActivePlayerIndex(address player) external view returns (uint256) {
    for (uint256 i = 0; i < players.length; i++) {
        if (players[i] == player) {
            return i;
        }
    }
    // Return a value indicating player not found
    return players.length;
}
