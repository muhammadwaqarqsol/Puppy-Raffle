# Findings

## Gas

### [G-2] Storage Variables in a Loop Should be Cached

Accessing storage variables within a loop incurs unnecessary gas costs. It's more efficient to cache storage variables in memory.

#### Recommendation
Cache storage variables in memory outside the loop to reduce gas consumption.

#### Action Taken
Storage variables have been cached in memory outside the loop to optimize gas usage.

