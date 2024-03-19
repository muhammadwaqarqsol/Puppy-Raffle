## [M-3] Unsafe Cast of `PuppyRaffle::fee` Loses Fees

### Description:
In the `PuppyRaffle::selectWinner` function, there is an unsafe type cast of a `uint256` to a `uint64`. This casting poses a risk of data truncation if the `uint256` value exceeds the maximum value representable by `uint64`. As a consequence, fees collected beyond this limit will be lost, leading to incorrect fee accounting within the contract.

### Impact:
The unsafe casting of fees in the `PuppyRaffle` contract results in potential loss of funds. If the total fees collected surpass the maximum value representable by `uint64`, which is approximately 18 ETH, the excess fees will not be correctly recorded, leaving them permanently stuck in the contract. This flaw compromises the integrity of fee collection and distribution mechanisms within the contract.

### Proof of Concept:
1. Conduct a raffle with slightly more than 18 ETH worth of fees collected.
2. Trigger the line that casts the fee as a `uint64`.
3. Observe incorrect updating of `totalFees` with a lower amount.
   
### Recommended Mitigation:
To address this issue, it is recommended to set `PuppyRaffle::totalFees` to a `uint256` instead of a `uint64` and remove the unsafe casting. While the current implementation might aim to save gas through storage packing, the potential gas savings are outweighed by the risks associated with data truncation. By utilizing a `uint256` for `totalFees` and eliminating the unsafe cast, the contract can ensure accurate fee accounting and prevent loss of funds.

```solidity
- uint64 public totalFees = 0;
+ uint256 public totalFees = 0;

### Mitigation Implementation:

function selectWinner() external {
    require(block.timestamp >= raffleStartTime + raffleDuration, "PuppyRaffle: Raffle not over");
    require(players.length >= 4, "PuppyRaffle: Need at least 4 players");

    uint256 winnerIndex = uint256(keccak256(abi.encodePacked(msg.sender, block.timestamp, block.difficulty))) % players.length;
    address winner = players[winnerIndex];

    uint256 totalAmountCollected = players.length * entranceFee;
    uint256 prizePool = (totalAmountCollected * 80) / 100;
    uint256 fee = (totalAmountCollected * 20) / 100;

    totalFees = totalFees + fee;
}
```
y implementing these changes, the PuppyRaffle contract can ensure accurate fee tracking and prevent potential loss of funds due to data truncation during type casting.
