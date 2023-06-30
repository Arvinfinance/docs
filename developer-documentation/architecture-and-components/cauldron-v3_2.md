# Cauldron V3\_2

The Cauldron V3\_2 is an incremental update to the V3 cauldron enabling the use of more powerful swappers, compatible with aggregators, like 0x. The new SwapperV2 need calldata given by the aggregator to work and therefore ask this as an input on the liquidate function, which uses a swapper.

### ✨ liquidate | Updated in V3\_2

```solidity
function liquidate(
        address[] calldata users,
        uint256[] calldata maxBorrowParts,
        address to,
        ISwapperV2 swapper,
        bytes calldata swapperData
    ) public
```

**Parameters**

| Name           | Type        | Text                                                                                                              |
| -------------- | ----------- | ----------------------------------------------------------------------------------------------------------------- |
| users          | `address[]` | Array of user addresses                                                                                           |
| maxBorrowParts | `uint256[]` | `A one-to-one mapping to users, contains maximum (partial) borrow amounts (to liquidate) of the respective user.` |
| to             | `address`   | `Address of the receiver in open liquidations if swapper is zero.`                                                |
| swapper        | `address`   | Address of the swapper, conforming to ISwapperV2.                                                                 |
| swapperData    | `bytes`     | CallData to be passed to the swapper.                                                                             |

**​**
