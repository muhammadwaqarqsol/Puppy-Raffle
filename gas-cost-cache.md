# Findings

## Gas

### [G-2] Storage Variables in a Loop Should be Cached

#### Description
Accessing storage variables within a loop incurs unnecessary gas costs. In the `enterRaffle` function of `PuppyRaffle.sol`, the contract checks for duplicates in an inefficient manner, resulting in increased gas consumption due to multiple reads from storage.

#### Recommendation
To optimize gas usage, cache storage variables in memory outside the loop. This reduces gas consumption by minimizing the number of times storage is accessed.

#### Code Changes
```diff
// Cache players.length in memory
+ uint256 playersLength = players.length;

// Optimize loop conditions
- for (uint256 i = 0; i < players.length - 1; i++) {
+ for (uint256 i = 0; i < playersLength - 1; i++) {

// Optimize inner loop condition
-    for (uint256 j = i + 1; j < players.length; j++) {
+    for (uint256 j = i + 1; j < playersLength; j++) {
      require(players[i] != players[j], "PuppyRaffle: Duplicate player");
.
```
#### Action Taken
Storage variables have been cached in memory outside the loop to optimize gas usage.

