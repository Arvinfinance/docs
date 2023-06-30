# Cauldron V2

Second version of the cauldrons, which changed the way fees and liquidations are accounted. The way the user interacts with it hasn't changed and no new functions were introduced. It is deployed directly and then used as a masterContract to deploy each market, as clones, following the minimal proxy pattern.

### constructor nonpayable (address)

This will create the masterContract that will be used by all the clones (markets).

| Parameter            | Description                 |
| -------------------- | --------------------------- |
| bentoBox\_           | The address of the BentoBox |
| magicInternetMoney\_ | The address of IN.          |

## Write Functions

### accrue

```solidity
function accrue() public
```

Accrues the interest on the borrowed tokens and handles the accumulation of fees.

### updateExchangeRate

```solidity
function updateExchangeRate() public returns (bool updated, uint256 rate)
```

Gets the exchange rate, ie how much collateral to buy 1e18 asset. Invoked if needed since Oracle queries can be expensive.

**Returns**

| Name      | Type    | Description                                                          |
| --------- | ------- | -------------------------------------------------------------------- |
| `updated` | bool    | boolean determining if the exchange rate has been updated yet or not |
| `rate`    | uint256 | the new exchange rate that was fetched                               |

### addCollateral

```solidity
function addCollateral(
        address to,
        bool skim,
        uint256 share
    ) public
```

Adds share amount of collateral from msg.sender to the account to.

**Parameters**

| Name                                                                 | Type    | Description                                                                    |
| -------------------------------------------------------------------- | ------- | ------------------------------------------------------------------------------ |
| `to`                                                                 | address | receiver of the tokens                                                         |
| `skim`                                                               | bool    | \`True if the amount should be skimmed from the deposit balance of msg.sender. |
| False if tokens from msg.sender in bentoBox should be transferred.\` |         |                                                                                |
| `share`                                                              | uint256 | `amount of shares to add for to`                                               |

### removeCollateral

```solidity
function removeCollateral(address to, uint256 share) public solvent
```

Calls \_removeCollateral, which removes the amount share of collateral and transfers it to the account to.

**Parameters**

| Name    | Type    | Description              |
| ------- | ------- | ------------------------ |
| `to`    | address | receiver of the shares   |
| `share` | uint256 | amount of shares to send |

### borrow

```solidity
function borrow(address to, uint256 amount) public solvent returns (uint256 part, uint256 share)
```

Calls \_borrow, which allows the sender to borrow amount and transfer to to.

**Parameters**

| Name     | Type    | Description                     |
| -------- | ------- | ------------------------------- |
| `to`     | address | receiver of the borrowed assets |
| `amount` | uint256 | amount of assets to borrow      |

**Returns**

| Name    | Type    | Description                          |
| ------- | ------- | ------------------------------------ |
| `part`  | uint256 | total part of debt held by borrowers |
| `share` | uint256 | total amount in shares borrowed      |

### repay

```solidity
function repay(
        address to,
        bool skim,
        uint256 part
    ) public returns (uint256 amount)
```

Calls \_repay, which repays a loan.

**Parameters**

| Name   | Type    | Description                                                             |
| ------ | ------- | ----------------------------------------------------------------------- |
| `to`   | address | address of user payment should go to                                    |
| `skim` | bool    | true if amount should be skimmed from the deposit balance of msg.sender |
| `part` | uint256 | amount to repay                                                         |

**Returns**

| Name     | Type    | Description          |
| -------- | ------- | -------------------- |
| `amount` | uint256 | total amount repayed |

### cook

```solidity
function cook(
        uint8[] calldata actions,
        uint256[] calldata values,
        bytes[] calldata datas
    ) external payable returns (uint256 value1, uint256 value2)
```

Executes a set of actions and allows composability (contract calls) to other contracts. The cook function allows to bundle functionality within one contract call while passing return values from one call to the next one. Actions are defined by a numeric identifier and can return two values, value1 and value2 to the next function. The input arrays actions, values and datas define the sequential actions. In the Value array the ether value of a call may be defined. Whereas calling functions like borrow that have the solvent modifier requires solvency at the end of the function, solvency only needs to be guaranteed at the end of the cook function, thereby allowing more complicated operations such as leveraging within one call. For certain parameters either an external value can be passed in or the identifier USEVALUE1 (-1) or USE\_VALUE2 (-2) to access either of the local variables. The following variables are marked in the table below. If an action returns one value it is saved as value1, if two are returned they are saved as value1 and value2 respectively. Any action can access these values during the whole duration of the cook call. The call data for the actions is ABI encoded as listed below.

| Action Name                       | ID | parameter names                                      | ABI encoding                                    | returnValues        |
| --------------------------------- | -- | ---------------------------------------------------- | ----------------------------------------------- | ------------------- |
| ACTION\_REPAY                     | 2  | share, to, skim                                      | int256, address, bool                           | ​                   |
| ACTION\_REMOVE\_COLLATERAL        | 4  | fraction, to                                         | int256, address                                 | ​                   |
| ACTION\_BORROW                    | 5  | amount, to                                           | int256, address                                 | part, share         |
| ACTION\_GET\_REPAY\_SHARE         | 6  | part                                                 | int256                                          | ​                   |
| ACTION\_GET\_REPAY\_PART          | 7  | amount                                               | int256                                          | ​                   |
| ACTION\_ACCRUE                    | 8  | ​                                                    | ​                                               | ​                   |
| ACTION\_ADD\_COLLATERAL           | 10 | share, to, skim                                      | int256, address, bool                           | ​                   |
| ACTION\_UPDATE\_EXCHANGE\_RATE    | 11 | must\_update, minRate, maxRate                       | bool, uint256, uint256                          | ​                   |
| ACTION\_BENTO\_DEPOSIT            | 20 | token, to, amount, share                             | IERC20, address, int256, int256                 | amountOut, shareOut |
| ACTION\_BENTO\_WITHDRAW           | 21 | token, to, amount, share                             | IERC20, address, int256, int256                 | amountOut, shareOut |
| ACTION\_BENTO\_TRANSFER           | 22 | token, to, share                                     | IERC20, address, int256                         | ​                   |
| ACTION\_BENTO\_TRANSFER\_MULTIPLE | 23 | token, tos, shares                                   | IERC20, address\[], uint256\[]                  | ​                   |
| ACTION\_BENTO\_SETAPPROVAL        | 24 | user, \_masterContract, approved, v, r, s            | address, address, bool, uint8, bytes32, bytes32 | ​                   |
| ACTION\_CALL                      | 30 | callee, callData, useValue1, useValue2, returnValues | address, bytes, bool, bool, uint8               | ​                   |

```solidity
// Functions that need accrue to be called
uint8 internal constant ACTION_REPAY = 2;
uint8 internal constant ACTION_REMOVE_COLLATERAL = 4;
uint8 internal constant ACTION_BORROW = 5;
uint8 internal constant ACTION_GET_REPAY_SHARE = 6;
uint8 internal constant ACTION_GET_REPAY_PART = 7;
uint8 internal constant ACTION_ACCRUE = 8;
​
// Functions that don't need accrue to be called
uint8 internal constant ACTION_ADD_COLLATERAL = 10;
uint8 internal constant ACTION_UPDATE_EXCHANGE_RATE = 11;
​
// Function on BentoBox
uint8 internal constant ACTION_BENTO_DEPOSIT = 20;
uint8 internal constant ACTION_BENTO_WITHDRAW = 21;
uint8 internal constant ACTION_BENTO_TRANSFER = 22;
uint8 internal constant ACTION_BENTO_TRANSFER_MULTIPLE = 23;
uint8 internal constant ACTION_BENTO_SETAPPROVAL = 24;
​
// Any external call (except to BentoBox)
uint8 internal constant ACTION_CALL = 30;
​
int256 internal constant USE_VALUE1 = -1;
int256 internal constant USE_VALUE2 = -2;
```

**Parameters**

| Name      | Type       | Description                                                                           |
| --------- | ---------- | ------------------------------------------------------------------------------------- |
| `actions` | uint8\[]   | array with sequence of actions to execute                                             |
| `values`  | uint256\[] | `one-to-one mapped array to actions, ETH amounts to send along with the actions`      |
| `datas`   | bytes\[]   | `one-to-one mapped array to actions, contains abi encoded data of function arguments` |

**Returns**

| Name     | Type    | Description                                                                                               |
| -------- | ------- | --------------------------------------------------------------------------------------------------------- |
| `value1` | uint256 | may contain first positioned return value of last executed action (if applicable)                         |
| `value2` | uint256 | may contain second positioned return value of last executed action which returns 2 values (if applicable) |

### liquidate

```solidity
function liquidate(
        address[] calldata users,
        uint256[] calldata maxBorrowParts,
        address to,
        ISwapper swapper,
    ) public
```

Handles the liquidation of users' balances once the users' amount of collateral is too low. **Parameters**

| Name             | Type       | Description                                                                                                    |
| ---------------- | ---------- | -------------------------------------------------------------------------------------------------------------- |
| `users`          | address\[] | array of user addresses                                                                                        |
| `maxBorrowParts` | uint256\[] | `one-to-one mapping to users, contains maximum (partial) borrow amounts (to liquidate) of the respective user` |
| `to`             | address    | `address of the receiver in open liquidations if swapper is zero`                                              |
| `swapper`        | ISwapper   | `contract address of the ISwapper implementation, swappers are restricted for closed liquidations`             |

### withdrawFees

```solidity
function withdrawFees() public
```

Withdraw the fees accumulated to the feeTo address.

### setFeeTo

```solidity
function setFeeTo(address newFeeTo) public onlyOwner
```

Sets the beneficiary of fees accrued in liquidations. Can only be called by the owner of the contract.

**Parameters**

| Name       | Type    | Description                |
| ---------- | ------- | -------------------------- |
| `newFeeTo` | address | address of the beneficiary |

### reduceSupply

```solidity
function reduceSupply(uint256 amount) public
```

Reduces the supply of IN **Parameters**

| Name   | Type    | Description                                           |
| ------ | ------- | ----------------------------------------------------- |
| amount | uint256 | amount to reduce supply by (in native representation) |

## View Functions

### BORROW\_OPENING\_FEE

Returns the opening fee (charged instantly, added to user debt upon borrowing) in Basis Points.

### COLLATERIZATION\_RATE

Returns the Collateralization Rate (maximum % borrowable with this collateral) in Basis Points. eg: 75000 = 75%

### LIQUIDATION\_MULTIPLIER

Returns the liquidation fee in Basis Points. eg: 112000 would add 12% (it's a multiplier, so the value is 112%)

### accrueInfo​​

**Return values**

| Name                  | Type      | Description                                                  |
| --------------------- | --------- | ------------------------------------------------------------ |
| lastAccrued           | `uint64`  | Timestamp of the last accrue() call.                         |
| feesEarned            | `uint128` | Fees accrue between the last withdrawal and the last accrue. |
| INTEREST\_PER\_SECOND | `uint64`  | ​                                                            |

### bentoBox

**Return values**

| Name                                 | Type      | Description                                          |
| ------------------------------------ | --------- | ---------------------------------------------------- |
| /                                    | `address` | Address of the bentoBox the cauldron is deployed on. |
| Set by constructor of masterContract |           |                                                      |

### collateral

**Return values**

| Name | Type      | Description                                      |
| ---- | --------- | ------------------------------------------------ |
| /    | `address` | Address of the collateral used by this cauldron. |

### exchangeRate

**Return values**

| Name | Type      | Description                   |
| ---- | --------- | ----------------------------- |
| /    | `uint256` | Current cached exchange rate. |

### feeTo​​

**Return values**

| Name | Type      | Text                                                                                                           |
| ---- | --------- | -------------------------------------------------------------------------------------------------------------- |
| /    | `address` | Recipient of the Fees (controlled by masterContract, which is why regular cauldrons have a feeTo zero address) |

### magicInternetMoney

**Return values**

| Name | Type      | Description                                         |
| ---- | --------- | --------------------------------------------------- |
| /    | `address` | Address of IN, set by constructor of masterContract |

### masterContract

**Return values**

| Name | Type      | Description             |
| ---- | --------- | ----------------------- |
| /    | `address` | masterContract address. |

### oracle​​

**Return values**

| Name | Type      | Description     |
| ---- | --------- | --------------- |
| /    | `address` | Oracle Address. |

### oracleData​​

**Return values**

| Name | Type    | Description                   |
| ---- | ------- | ----------------------------- |
| /    | `bytes` | Oracle data used to query it. |

#### owner

**Return values**

| Name                                                                                         | Type      | Text                  |
| -------------------------------------------------------------------------------------------- | --------- | --------------------- |
| /                                                                                            | `address` | Owner of the Cauldron |
| (controlled by masterContract, which is why regular cauldrons have zeroAddress as an owner). |           |                       |

### pendingOwner​​

When transferring ownership, the future owner is pending until they claims ownership.

**Return values**

| Name | Type      | Description                   |
| ---- | --------- | ----------------------------- |
| /    | `address` | Address of the pending owner. |

### totalBorrow​​

Returns the total amounts borrowed from the cauldron.

**Return values**

| Name    | Type      | Description                          |
| ------- | --------- | ------------------------------------ |
| elastic | `uint128` | Amount of IN borrowed by users.      |
| base    | `uint128` | Amount of borrowParts held by users. |

### totalCollateralShare​​

Returns the amount (in shares) of the token collateral used as collateral in this cauldron.

**Return values**

| Name | Type      | Description                                                 |
| ---- | --------- | ----------------------------------------------------------- |
| /    | `uint256` | Amount of shares of collateral deposited into the Cauldron. |

### userBorrowPart

Returns the amount of borrowParts held by a user. To convert that amount into a numerical IN debt amount, you must do totalBorrow.elastic / totalBorrow.base \* borrowParts.

**Parameters**

| Name | Type      | Description   |
| ---- | --------- | ------------- |
| /    | `address` | User address. |

**Return values**

| Name | Type      | Description                 |
| ---- | --------- | --------------------------- |
| /    | `uint256` | Amount of borrowParts held. |

### userCollateralShare​​

Amount (in shares) of the token collateral used as collateral by the user.

**Parameters**

| Name | Type      | Description   |
| ---- | --------- | ------------- |
| /    | `address` | User address. |

**Return values**

| Name | Type      | Description                     |
| ---- | --------- | ------------------------------- |
| /    | `uint256` | Shares of collateral deposited. |
