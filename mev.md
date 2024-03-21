# Findings

## MEV Vulnerability in refund function

### Description
The `refund` function in the `PuppyRaffle.sol` contract is vulnerable to Miner Extractable Value (MEV). MEV refers to the ability of miners to manipulate the order or inclusion of transactions in blocks to their advantage, potentially leading to undesirable outcomes such as front-running or sandwich attacks.

### Recommendation
Given the complexity and potential severity of MEV vulnerabilities, it is crucial to address them promptly. Consider implementing mitigation strategies such as using designated relayers, adjusting transaction ordering mechanisms, or deploying MEV-resistant protocols.

### Action Taken
The MEV vulnerability in the `refund` function has been flagged for further investigation and mitigation measures will be implemented accordingly. It is important to thoroughly understand MEV and its implications to ensure the security and integrity of the smart contract.

