# Findings

## [H-2] Weak Randomness in `PuppyRaffle::selectWinner` allows users to influence or predict the winner and influence or predict the winning puppy

### Description
Hashing `msg.sender`, `block.timestamp`, and `block.difficulty` together creates a predictable final number, which does not provide sufficient randomness for selecting a winner in the `selectWinner` function of `PuppyRaffle.sol`. Malicious users can manipulate these values or know them ahead of time to choose the winner of the raffle themselves.

**Note:** This vulnerability also exposes the protocol to front-running attacks, where users can call `refund` if they are not the predicted winner.

### Impact
Any user can influence the winner of the raffle, winning the prize pool and selecting the "rarest" puppy. This compromises the integrity of the raffle and can lead to gas wars to determine the winner, rendering the entire process worthless.

### Proof of Concept
1. Validators can anticipate the values of `block.timestamp` and `block.difficulty` and use them to predict the winner.
2. Users can manipulate their `msg.sender` value to ensure they are selected as the winner.
3. Users can revert their `selectWinner` transaction if they disagree with the winner or the resulting puppy.

Using on-chain values as a randomness seed is a well-documented attack vector in the blockchain space.

### Recommended Mitigation
Consider implementing a cryptographically provable random number generator such as [Chainlink VRF](https://docs.chain.link/vrf) to ensure secure and unbiased randomness in selecting the winner.

