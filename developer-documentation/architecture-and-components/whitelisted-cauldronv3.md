# Whitelisted CauldronV3

Whitelisted CauldronV3 is an update of the V3 cauldron that allows Arvin to whitelist certain addresses to borrow up to a certain amount. It uses a Whitelister contract that contains the root hash of a merkleTree of addresses and amounts and once called with the correct proof, stores the maxBorrow amount, for the cauldron to check against while borrowing. You can find the full contract of the cauldron here and of the whitelister here. ​

### ✨ changeWhitelister | Specific to Whitelisted

```solidity
    function changeWhitelister(IWhitelister newWhiteLister) public onlyMasterContractOwner
```

**Parameters**

| Name           | Type      |
| -------------- | --------- |
| newWhiteLister | `address` |

**​**
