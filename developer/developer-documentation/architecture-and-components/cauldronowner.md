# CauldronOwner

### `Constructor​​`

#### `nonpayable​​`

**Parameters**

| Name       | Type      |
| ---------- | --------- |
| \_treasury | `address` |
| \_IN       | `address` |

## Functions​​

* ​changeBorrowLimit​
* ​changeInterestRate​
* ​claimOwnership​
* ​deprecated​
* ​execute​
* ​IN​
* ​operators​
* ​owner​
* ​pendingOwner​
* ​reduceCompletely​
* ​reduceSupply​
* ​setAllowedSupplyReducer​
* ​setBlacklistedCallee​
* ​setDeprecated​
* ​setFeeTo​
* ​setOperator​
* ​setTreasury​
* ​transferMasterContractOwnership​
* ​transferOwnership​
* ​treasury​
* ​withdrawINToTreasury​

​

### `changeBorrowLimit nonpayable​​`

**Parameters**

| Name           | Type      |
| -------------- | --------- |
| cauldron       | `address` |
| newBorrowLimit | `uint128` |
| perAddressPart | `uint128` |

**Return values** No return values for this function.

### `changeInterestRate nonpayable​​`

**Parameters**

| Name            | Type      |
| --------------- | --------- |
| cauldron        | `address` |
| newInterestRate | `uint64`  |

**Return values** No return values for this function.

### `claimOwnership nonpayable​​`

**Parameters**

No parameters for this function.

**Return values**

No return values for this function.

### `deprecated view​​`

**Parameters**

| Name | Type      |
| ---- | --------- |
| /    | `address` |

**Return values**

| Name | Type   |
| ---- | ------ |
| /    | `bool` |

### `execute nonpayable​​`

**Parameters**

| Name  | Type      |
| ----- | --------- |
| to    | `address` |
| value | `uint256` |
| data  | `bytes`   |

**Return values**

| Name    | Type    |
| ------- | ------- |
| success | `bool`  |
| result  | `bytes` |

### `IN view​​`

**Parameters**

No parameters for this function.

**Return values**

| Name | Type      |
| ---- | --------- |
| /    | `address` |

### `operators view​​`

**Parameters**

| Name | Type      |
| ---- | --------- |
| /    | `address` |

**Return values**

| Name | Type   |
| ---- | ------ |
| /    | `bool` |

### `owner view​​`

**Parameters**

No parameters for this function.

**Return values**

| Name | Type      |
| ---- | --------- |
| /    | `address` |

### `pendingOwner view​​`

**Parameters**

No parameters for this function.

**Return values**

| Name | Type      |
| ---- | --------- |
| /    | `address` |

### `reduceCompletely nonpayable​​`

**Parameters**

| Name     | Type      |
| -------- | --------- |
| cauldron | `address` |

**Return values**

No return values for this function.

### `reduceSupply nonpayable​​`

**Parameters**

| Name     | Type      |
| -------- | --------- |
| cauldron | `address` |
| amount   | `uint256` |

**Return values**

No return values for this function.

### `setAllowedSupplyReducer nonpayable​​`

**Parameters**

| Name     | Type      |
| -------- | --------- |
| cauldron | `address` |
| account  | `address` |
| allowed  | `bool`    |

**Return values**

No return values for this function.

### `setBlacklistedCallee nonpayable​​`

**Parameters**

| Name        | Type      |
| ----------- | --------- |
| cauldron    | `address` |
| callee      | `address` |
| blacklisted | `bool`    |

**Return values**

No return values for this function.

### `setDeprecated nonpayable​​`

**Parameters**

| Name         | Type      |
| ------------ | --------- |
| cauldron     | `address` |
| \_deprecated | `bool`    |

**Return values**

No return values for this function.

### `setFeeTo nonpayable​​`

**Parameters**

| Name     | Type      |
| -------- | --------- |
| cauldron | `address` |
| newFeeTo | `address` |

**Return values**

No return values for this function.

### `setOperator nonpayable​​`

**Parameters**

| Name     | Type      |
| -------- | --------- |
| operator | `address` |
| enabled  | `bool`    |

**Return values**

No return values for this function.

### `setTreasury nonpayable​​`

**Parameters**

| Name       | Type      |
| ---------- | --------- |
| \_treasury | `address` |

**Return values**

No return values for this function.

### `transferMasterContractOwnership nonpayable​​`

**Parameters**

| Name           | Type      |
| -------------- | --------- |
| masterContract | `address` |
| newOwner       | `address` |

**Return values**

No return values for this function.

### `transferOwnership nonpayable​​`

**Parameters**

| Name     | Type      |
| -------- | --------- |
| newOwner | `address` |
| direct   | `bool`    |
| renounce | `bool`    |

**Return values**

No return values for this function.

### `treasury view​​`

**Parameters**

No parameters for this function.

**Return values**

| Name | Type      |
| ---- | --------- |
| /    | `address` |

### `withdrawINToTreasury nonpayable​​`

**Parameters**

| Name     | Type      |
| -------- | --------- |
| bentoBox | `address` |
| share    | `uint256` |

**Return values**

No return values for this function.

## Events (for EVM logging)​​

* ​LogDeprecated​
* ​LogOperatorChanged​
* ​LogTreasuryChanged​
* ​OwnershipTransferred​

​

### `LogDeprecated​​`

**Parameters**

| Name     | Type      | Indexed |
| -------- | --------- | ------- |
| cauldron | `address` | `true`  |
| previous | `bool`    | `false` |
| current  | `bool`    | `false` |

### `LogOperatorChanged​​`

**Parameters**

| Name     | Type      | Indexed |
| -------- | --------- | ------- |
| operator | `address` | `true`  |
| previous | `bool`    | `false` |
| current  | `bool`    | `false` |

### `LogTreasuryChanged​​`

**Parameters**

| Name     | Type      | Indexed |
| -------- | --------- | ------- |
| previous | `address` | `true`  |
| current  | `address` | `true`  |

### `OwnershipTransferred​​`

**Parameters**

| Name          | Type      | Indexed |
| ------------- | --------- | ------- |
| previousOwner | `address` | `true`  |
| newOwner      | `address` | `true`  |
