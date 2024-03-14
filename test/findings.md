Denial of Service attack

### [M-#] Looping through players to check duplicates  in `PuppyRaffle:enterRaffle` is a potential denail of service attack(DOS), incrementing gas costs for future entrants

IMPACT: MEDIUM
LIKELIHOOD: MEDIUM

**Description:** The `PuppyRaffle:enterRaffle` function loops through array of the `players` for checking duplicates. However, the longer the `PuppyRaffle:enterRaffle` loops through the more gas it willcost for new entrants. The more the cheks the new user make it higher gas cost. Every new address is a new check loop creator in `players` array. Also a frontRun attack can also be used on it. Which is another issue.

```javascript
//@audit DOS Attack
@> for (uint256 i = 0; i < players.length - 1; i++) {
            for (uint256 j = i + 1; j < players.length; j++) {
                require(players[i] != players[j], "PuppyRaffle: Duplicate player");
            }
        }
```

**Impact:** The gas costs for raffle entrants will greatly increased as more players enter Raffle. Discouraging future entrants due to high gas fees. 

An attacker might make the `PuppyRaffle:entrants` array so big, that no one else enters, guaranteeing themselves the win.

**Proof of Concepts:**

If we have two set of 100 players. The gas costs will be as such:
- 1st 100 players: ~6252048
- Next 100 Players: ~18068138

This is more than 3x expensive for the new entrants.

<details>
<summary>POC</summary>
Place the following test into the PuppyRaffleTest.t.sol

```javascript

    function testCanEnterRaffleUnlimitedAmountofTime()public{
        //setting gas to one so we get actual gas
        vm.txGasPrice(1);
        //enter 100 players
        uint256 playersNum=100;
        address[] memory players=new address[](playersNum);
        for(uint256 i=0; i<playersNum;i++){
            players[i]=address(i);
        }
        //gas costs
        uint256 gasStart=gasleft();
        puppyRaffle.enterRaffle{value:entranceFee*players.length}(players);
        uint256 gasEnd=gasleft();

        uint256 gasUsedfor100=(gasStart-gasEnd)*tx.gasprice;
        console.log("gas cost for 100 players",gasUsedfor100);

        //next 100 players
        address[] memory playerstwo=new address[](playersNum);
        for(uint256 i=0; i<playersNum;i++){
            playerstwo[i]=address(i+playersNum);
        }
        
         //gas costs
        uint256 gasStartsecond=gasleft();
        puppyRaffle.enterRaffle{value:entranceFee*players.length}(playerstwo);
        uint256 gasEndsecond=gasleft();

        uint256 gasUsedfor200=(gasStartsecond-gasEndsecond)*tx.gasprice;
        console.log("gas cost for another 200 players",gasUsedfor200);
        assert(gasUsedfor100 < gasUsedfor200);
     }
```
</details>



**Recommend Mitigation:**
There are few recommendations 
1. Consider allowing duplicates. user can make new wallets anyway so a duplicate checks doesn't prevent the same person from entering from multiple times.
2. Consider using mapping to check duplicates. This would allow constant time lookup of wheather a user has already enterred or not.

```diff

+add mapping (address=> uint256) public addresstoRaffleId;
+add uint256 public raffleId=0;


function enterRaffle(address[] memory newPlayers)public{
require(msg.value == entranceFee * newPlayers.length, "PuppyRaffle: Must send enough to enter raffle");
        for (uint256 i = 0; i < newPlayers.length; i++) {
            players.push(newPlayers[i]);
+            addresstoRaffleId[newPlayers[i]]=raffleId;
        }

-      Check for duplicates
+      //check for duplicates
+       for (uint256 i=0; i<newPlayers.length;i++){
+       require(addresstoRaffleId[newPlayers[i]]!=raffleId,"PuppyRaffle:Duplicate Player")
+       }

-  for (uint256 i = 0; i < players.length - 1; i++) {
-            for (uint256 j = i + 1; j < players.length; j++) {
-                require(players[i] != players[j], "PuppyRaffle: Duplicate player");
-            }
-        }
        emit RaffleEnter(newPlayers);
}
.
.
.
    function selectWinner()external{
+       raffleId=raffleId+1;
        require(block.timestamp >= raffleStartTime+raffleDuration,"PuppyRaffle: Raffle not found");
```

Alternatively, you could use [OpenZepplin's`EnumerableSet`library] (https://docs.openzeppelin.com/contracts/1.x/api/utils#EnumerableSet).