- INFO:Detectors:
- PuppyRaffle.withdrawFees() (src/PuppyRaffle.sol#189-201) sends eth to arbitrary user
        Dangerous calls:
        - (success) = feeAddress.call{value: feesToWithdraw}() (src/PuppyRaffle.sol#199)
        Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#functions-that-send-ether-to-arbitrary-destinations


- INFO:Detectors:
- PuppyRaffle.selectWinner() (src/PuppyRaffle.sol#131-186) uses a weak PRNG: "winnerIndex = uint256(keccak256(bytes)(abi.encodePacked(msg.sender,block.timestamp,block.difficulty))) % players.length (src/PuppyRaffle.sol#140-141)"
        Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#weak-PRNG


- INFO:Detectors:
- Base64.encode(bytes) (lib/base64/base64.sol#15-66) performs a multiplication on the result of a division:
        - encodedLen = 4 * ((data.length + 2) / 3) (lib/base64/base64.sol#22)
Base64.decode(string) (lib/base64/base64.sol#68-129) performs a multiplication on the result of a division:
        - decodedLen = (data.length / 4) * 3 (lib/base64/base64.sol#78)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#divide-before-multiply


- INFO:Detectors:
- PuppyRaffle.withdrawFees() (src/PuppyRaffle.sol#189-201) uses a dangerous strict equality:
        - require(bool,string)(address(this).balance == uint256(totalFees),PuppyRaffle: There are currently players active!) (src/PuppyRaffle.sol#195)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#dangerous-strict-equalities


- INFO:Detectors:
- Reentrancy in PuppyRaffle.refund(uint256) (src/PuppyRaffle.sol#99-109):
        External calls:
        - address(msg.sender).sendValue(entranceFee) (src/PuppyRaffle.sol#105)
        State variables written after the call(s):
        - players[playerIndex] = address(0) (src/PuppyRaffle.sol#107)
        PuppyRaffle.players (src/PuppyRaffle.sol#23) can be used in cross function reentrancies:
        - PuppyRaffle.enterRaffle(address[]) (src/PuppyRaffle.sol#80-95)
        - PuppyRaffle.getActivePlayerIndex(address) (src/PuppyRaffle.sol#114-123)
        - PuppyRaffle.players (src/PuppyRaffle.sol#23)
        - PuppyRaffle.refund(uint256) (src/PuppyRaffle.sol#99-109)
        - PuppyRaffle.selectWinner() (src/PuppyRaffle.sol#131-186)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-1


- INFO:Detectors:
- ERC721.tokenByIndex(uint256) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#180-183) ignores return value by (tokenId) = _tokenOwners.at(index) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#181)
- ERC721._mint(address,uint256) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#333-344) ignores return value by _holderTokens[to].add(tokenId) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#339)
- ERC721._mint(address,uint256) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#333-344) ignores return value by _tokenOwners.set(tokenId,to) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#341)
- ERC721._burn(uint256) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#356-374) ignores return value by _holderTokens[owner].remove(tokenId) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#369)
- ERC721._burn(uint256) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#356-374) ignores return value by _tokenOwners.remove(tokenId) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#371)
- ERC721._transfer(address,address,uint256) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#387-402) ignores return value by _holderTokens[from].remove(tokenId) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#396)
- ERC721._transfer(address,address,uint256) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#387-402) ignores return value by _holderTokens[to].add(tokenId) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#397)
- ERC721._transfer(address,address,uint256) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#387-402) ignores return value by _tokenOwners.set(tokenId,to) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#399)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#unused-return


- INFO:Detectors:
- PuppyRaffle.constructor(uint256,address,uint256)._feeAddress (src/PuppyRaffle.sol#61) lacks a zero-check on :
                - feeAddress = _feeAddress (src/PuppyRaffle.sol#63)
PuppyRaffle.changeFeeAddress(address).newFeeAddress (src/PuppyRaffle.sol#205) lacks a zero-check on :
                - feeAddress = newFeeAddress (src/PuppyRaffle.sol#206)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#missing-zero-address-validation


- INFO:Detectors:
- Reentrancy in PuppyRaffle.refund(uint256) (src/PuppyRaffle.sol#99-109):
        External calls:
        - address(msg.sender).sendValue(entranceFee) (src/PuppyRaffle.sol#105)
        Event emitted after the call(s):
        - RaffleRefunded(playerAddress) (src/PuppyRaffle.sol#108)
- Reentrancy in PuppyRaffle.selectWinner() (src/PuppyRaffle.sol#131-186):
        External calls:
        - (success) = winner.call{value: prizePool}() (src/PuppyRaffle.sol#183)
        - _safeMint(winner,tokenId) (src/PuppyRaffle.sol#185)
                - returndata = to.functionCall(abi.encodeWithSelector(IERC721Receiver(to).onERC721Received.selector,_msgSender(),from,tokenId,_data),ERC721: transfer to non ERC721Receiver implementer) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#441-447)
                - (success,returndata) = target.call{value: value}(data) (lib/openzeppelin-contracts/contracts/utils/Address.sol#119)
        External calls sending eth:
        - (success) = winner.call{value: prizePool}() (src/PuppyRaffle.sol#183)
        - _safeMint(winner,tokenId) (src/PuppyRaffle.sol#185)
                - (success,returndata) = target.call{value: value}(data) (lib/openzeppelin-contracts/contracts/utils/Address.sol#119)
        Event emitted after the call(s):
        - Transfer(address(0),to,tokenId) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#343)
                - _safeMint(winner,tokenId) (src/PuppyRaffle.sol#185)
        Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#reentrancy-vulnerabilities-3


- INFO:Detectors:
- PuppyRaffle.selectWinner() (src/PuppyRaffle.sol#131-186) uses timestamp for comparisons
        Dangerous comparisons:
        - require(bool,string)(block.timestamp >= raffleStartTime + raffleDuration,PuppyRaffle: Raffle not over) (src/PuppyRaffle.sol#136)
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#block-timestamp


- INFO:Detectors:
- Base64.encode(bytes) (lib/base64/base64.sol#15-66) uses assembly
        - INLINE ASM (lib/base64/base64.sol#27-63)
  Base64.decode(string) (lib/base64/base64.sol#68-129) uses assembly
        - INLINE ASM (lib/base64/base64.sol#83-126)
  Address.isContract(address) (lib/openzeppelin-contracts/contracts/utils/Address.sol#26-35) uses assembly
        - INLINE ASM (lib/openzeppelin-contracts/contracts/utils/Address.sol#33)
  Address._verifyCallResult(bool,bytes,string) (lib/openzeppelin-contracts/contracts/utils/Address.sol#171-188) uses assembly
        - INLINE ASM (lib/openzeppelin-contracts/contracts/utils/Address.sol#180-183) 
  Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#assembly-usage

- INFO:Detectors:
- Different versions of Solidity are used:
        - Version used: ['>=0.6.0', '>=0.6.0<0.8.0', '>=0.6.2<0.8.0', '^0.7.6']
        - >=0.6.0 (lib/base64/base64.sol#3)
        - >=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/access/Ownable.sol#3)
        - >=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#3)
        - >=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/IERC721Receiver.sol#3)
        - >=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/introspection/ERC165.sol#3)
        - >=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/introspection/IERC165.sol#3)
        - >=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#3)
        - >=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/utils/Context.sol#3)
        - >=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/utils/EnumerableMap.sol#3)
        - >=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/utils/EnumerableSet.sol#3)
        - >=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/utils/Strings.sol#3)
        - >=0.6.2<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/IERC721.sol#3)
        - >=0.6.2<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/IERC721Enumerable.sol#3)
        - >=0.6.2<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/IERC721Metadata.sol#3)
        - >=0.6.2<0.8.0 (lib/openzeppelin-contracts/contracts/utils/Address.sol#3)
        - ^0.7.6 (src/PuppyRaffle.sol#2)
    Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#different-pragma-directives-are-used


- INFO:Detectors:
- Address.functionCall(address,bytes) (lib/openzeppelin-contracts/contracts/utils/Address.sol#79-81) is never used and should be removed

- Address.functionCallWithValue(address,bytes,uint256) (lib/openzeppelin-contracts/contracts/utils/Address.sol#104-106) is never used and should be removed

- Address.functionDelegateCall(address,bytes) (lib/openzeppelin-contracts/contracts/utils/Address.sol#153-155) is never used and should be removed
- Address.functionDelegateCall(address,bytes,string) (lib/openzeppelin-contracts/contracts/utils/Address.sol#163-169) is never used and should be removed

- Address.functionStaticCall(address,bytes) (lib/openzeppelin-contracts/contracts/utils/Address.sol#129-131) is never used and should be removed
- Address.functionStaticCall(address,bytes,string) (lib/openzeppelin-contracts/contracts/utils/Address.sol#139-145) is never used and should be removed
- Base64.decode(string) (lib/base64/base64.sol#68-129) is never used and should be removed
Context._msgData() (lib/openzeppelin-contracts/contracts/utils/Context.sol#20-23) is never used and should be removed
- ERC721._burn(uint256) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#356-374) is never used and should be removed
- ERC721._setBaseURI(string) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#421-423) is never used and should be removed
- ERC721._setTokenURI(uint256,string) (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#411-414) is never used and should be removed
- EnumerableMap._get(EnumerableMap.Map,bytes32) (lib/openzeppelin-contracts/contracts/utils/EnumerableMap.sol#163-167) is never used and should be removed
- EnumerableMap._remove(EnumerableMap.Map,bytes32) (lib/openzeppelin-contracts/contracts/utils/EnumerableMap.sol#81-113) is never used and should be removed
- EnumerableMap._tryGet(EnumerableMap.Map,bytes32) (lib/openzeppelin-contracts/contracts/utils/EnumerableMap.sol#150-154) is never used and should be removed
- EnumerableMap.get(EnumerableMap.UintToAddressMap,uint256) (lib/openzeppelin-contracts/contracts/utils/- - - EnumerableMap.sol#253-255) is never used and should be removed

- EnumerableMap.remove(EnumerableMap.UintToAddressMap,uint256) (lib/openzeppelin-contracts/contracts/utils/EnumerableMap.sol#203-205) is never used and should be removed

- EnumerableMap.tryGet(EnumerableMap.UintToAddressMap,uint256) (lib/openzeppelin-contracts/contracts/utils/EnumerableMap.sol#241-244) is never used and should be removed
- EnumerableSet.add(EnumerableSet.AddressSet,address) (lib/openzeppelin-contracts/contracts/utils/EnumerableSet.sol#201-203) is never used and should be removed
- EnumerableSet.add(EnumerableSet.Bytes32Set,bytes32) (lib/openzeppelin-contracts/contracts/utils/EnumerableSet.sol#147-149) is never used and should be removed
- EnumerableSet.at(EnumerableSet.AddressSet,uint256) (lib/openzeppelin-contracts/contracts/utils/EnumerableSet.sol#239-241) is never used and should be removed
- EnumerableSet.at(EnumerableSet.Bytes32Set,uint256) (lib/openzeppelin-contracts/contracts/utils/EnumerableSet.sol#185-187) is never used and should be removed
- EnumerableSet.contains(EnumerableSet.AddressSet,address) (lib/openzeppelin-contracts/contracts/utils/EnumerableSet.sol#218-220) is never used and should be removed
- EnumerableSet.contains(EnumerableSet.Bytes32Set,bytes32) (lib/openzeppelin-contracts/contracts/utils/EnumerableSet.sol#164-166) is never used and should be removed
- EnumerableSet.contains(EnumerableSet.UintSet,uint256) (lib/openzeppelin-contracts/contracts/utils/EnumerableSet.sol#273-275) is never used and should be removed
- EnumerableSet.length(EnumerableSet.AddressSet) (lib/openzeppelin-contracts/contracts/utils/EnumerableSet.sol#225-227) is never used and should be removed
- EnumerableSet.length(EnumerableSet.Bytes32Set) (lib/openzeppelin-contracts/contracts/utils/EnumerableSet.sol#171-173) is never used and should be removed
- EnumerableSet.remove(EnumerableSet.AddressSet,address) (lib/openzeppelin-contracts/contracts/utils/EnumerableSet.sol#211-213) is never used and should be removed
- EnumerableSet.remove(EnumerableSet.Bytes32Set,bytes32) (lib/openzeppelin-contracts/contracts/utils/EnumerableSet.sol#157-159) is never used and should be removed
- PuppyRaffle._isActivePlayer() (src/PuppyRaffle.sol#211-218) is never used and should be removed
- SafeMath.add(uint256,uint256) (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#85-89) is never used and should be removed
- SafeMath.div(uint256,uint256) (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#135-138) is never used and should be removed
- SafeMath.div(uint256,uint256,string) (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#190-193) is never used and should be removed
- SafeMath.mod(uint256,uint256) (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#152-155) is never used and should be removed
- SafeMath.mod(uint256,uint256,string) (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#210-213) is - never used and should be removed
- SafeMath.mul(uint256,uint256) (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#116-121) is never used and should be removed
- SafeMath.sub(uint256,uint256) (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#101-104) is never used and should be removed
- SafeMath.sub(uint256,uint256,string) (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#170-173) is never used and should be removed
- SafeMath.tryAdd(uint256,uint256) (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#24-28) is never used and should be removed
- SafeMath.tryDiv(uint256,uint256) (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#60-63) is never used and should be removed
- SafeMath.tryMod(uint256,uint256) (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#70-73) is never used and should be removed
- SafeMath.tryMul(uint256,uint256) (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#45-53) is never used and should be removed
- SafeMath.trySub(uint256,uint256) (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#35-38) is never used and should be removed
- Strings.toString(uint256) (lib/openzeppelin-contracts/contracts/utils/Strings.sol#12-33) is never used and should be removed
      Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#dead-code

- INFO:Detectors:
- Pragma version>=0.6.0 (lib/base64/base64.sol#3) allows old versions
- Pragma version>=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/access/Ownable.sol#3) is too complex
- Pragma version>=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#3) is too complex
- Pragma version>=0.6.2<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/IERC721.sol#3) is too complex
- Pragma version>=0.6.2<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/IERC721Enumerable.sol#3) is too complex
- Pragma version>=0.6.2<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/IERC721Metadata.sol#3) is too complex
- Pragma version>=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/token/ERC721/IERC721Receiver.sol#3) is too complex
- Pragma version>=0.6.2<0.8.0 (lib/openzeppelin-contracts/contracts/utils/Address.sol#3) is too complex
- Pragma version>=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/introspection/ERC165.sol#3) is too complex
- Pragma version>=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/introspection/IERC165.sol#3) is too complex
- Pragma version>=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/math/SafeMath.sol#3) is too complex
- Pragma version>=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/utils/Context.sol#3) is too complex
- Pragma version>=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/utils/EnumerableMap.sol#3) is too complex
- Pragma version>=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/utils/EnumerableSet.sol#3) is too complex
- Pragma version>=0.6.0<0.8.0 (lib/openzeppelin-contracts/contracts/utils/Strings.sol#3) is too complex
- Pragma version^0.7.6 (src/PuppyRaffle.sol#2) allows old versions
- solc-0.7.6 is not recommended for deployment
     Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#incorrect-versions-of-solidity



- INFO:Detectors:
- Low level call in Address.sendValue(address,uint256) (lib/openzeppelin-contracts/contracts/utils/Address.sol#53-59):
        - (success) = recipient.call{value: amount}() (lib/openzeppelin-contracts/contracts/utils/Address.sol#57)
- Low level call in Address.functionCallWithValue(address,bytes,uint256,string) (lib/openzeppelin-contracts/contracts/utils/Address.sol#114-121):
        - (success,returndata) = target.call{value: value}(data) (lib/openzeppelin-contracts/contracts/utils/Address.sol#119)
- Low level call in Address.functionStaticCall(address,bytes,string) (lib/openzeppelin-contracts/contracts/utils/Address.sol#139-145):
        - (success,returndata) = target.staticcall(data) (lib/openzeppelin-contracts/contracts/utils/Address.sol#143)
- Low level call in Address.functionDelegateCall(address,bytes,string) (lib/openzeppelin-contracts/contracts/utils/Address.sol#163-169):
        - (success,returndata) = target.delegatecall(data) (lib/openzeppelin-contracts/contracts/utils/Address.sol#167)
- Low level call in PuppyRaffle.selectWinner() (src/PuppyRaffle.sol#131-186):
        - (success) = winner.call{value: prizePool}() (src/PuppyRaffle.sol#183)
- Low level call in PuppyRaffle.withdrawFees() (src/PuppyRaffle.sol#189-201):
        - (success) = feeAddress.call{value: feesToWithdraw}() (src/PuppyRaffle.sol#199)
        Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#low-level-calls


- INFO:Detectors:
- Parameter Base64.decode(string)._data (lib/base64/base64.sol#68) is not in mixedCase
- Parameter ERC721.safeTransferFrom(address,address,uint256,bytes)._data (lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol#245) is not in mixedCase
       Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#conformance-to-solidity-naming-conventions


- INFO:Detectors:
- Redundant expression "this (lib/openzeppelin-contracts/contracts/utils/Context.sol#21)" inContext (lib/openzeppelin-contracts/contracts/utils/Context.sol#15-24)
- Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#redundant-statements



- INFO:Detectors:
- Variable Base64.TABLE_DECODE (lib/base64/base64.sol#10-13) is too similar to Base64.TABLE_ENCODE (lib/base64/base64.sol#9)
- Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#variable-names-too-similar


- INFO:Detectors:
- Loop condition i < players.length (src/PuppyRaffle.sol#115) should use cached array length instead of referencing `length` member of the storage array.
- Loop condition i < players.length (src/PuppyRaffle.sol#212) should use cached array length instead of referencing `length` member of the storage array.
- Loop condition j < players.length (src/PuppyRaffle.sol#90) should use cached array length instead of - referencing `length` member of the storage array.
       Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#cache-array-length

- INFO:Detectors:
- PuppyRaffle.commonImageUri (src/PuppyRaffle.sol#39) should be constant
- PuppyRaffle.legendaryImageUri (src/PuppyRaffle.sol#49) should be constant
- PuppyRaffle.rareImageUri (src/PuppyRaffle.sol#44) should be constant
      Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#state-variables-that-could-be-declared-constant


- INFO:Detectors:
- PuppyRaffle.raffleDuration (src/PuppyRaffle.sol#25) should be immutable
Reference: https://github.com/crytic/slither/wiki/Detector-Documentation#state-variables-that-could-be-declared-immutable

- INFO:Slither:src/ analyzed (16 contracts with 94 detectors), 101 result(s) found