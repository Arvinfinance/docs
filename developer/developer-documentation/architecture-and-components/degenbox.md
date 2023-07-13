# DegenBox

### `Constructor​ nonpayable​​`

**Parameters**

| Name        | Type      | Description      |
| ----------- | --------- | ---------------- |
| wethToken\_ | `address` | address of WETH. |

## Functions

​​**View Functions**

* ​DOMAIN\_SEPARATOR​
* ​balanceOf​
* ​masterContractApproved​
* ​masterContractOf​
* ​nonces​
* ​owner​
* ​pendingOwner​
* ​pendingStrategy​
* ​strategy​
* ​strategyData​
* ​toAmount​
* ​toShare​
* ​totals​
* ​whitelistedMasterContracts​

**Write Functions**

* ​batch​
* ​batchFlashLoan​
* ​claimOwnership​
* ​deploy​
* ​deposit​
* ​flashLoan​
* ​harvest​
* ​permitToken​
* ​registerProtocol​
* ​setMasterContractApproval​
* ​setStrategy​
* ​setStrategyTargetPercentage​
* ​transfer​
* ​transferMultiple​
* ​transferOwnership​
* ​whitelistMasterContract​
* ​withdraw​

​

### `DOMAIN_SEPARATOR | view​​`

**Parameters** No parameters for this function. **Return values**

| Name | Type      |
| ---- | --------- |
| /    | `bytes32` |

### `balanceOf | view​​`

```solidity
mapping(IERC20 => mapping(address => uint256)) public balanceOf
```

**Parameters**

| Name | Type      | Description                          |
| ---- | --------- | ------------------------------------ |
| /    | `address` | Token to check                       |
| /    | `address` | The address which balance is checked |

**Return values**

| Name | Type      | Description                          |
| ---- | --------- | ------------------------------------ |
| /    | `uint256` | Shares of 'token' owned by 'address' |

### `batch | payable​​`

```solidity
function batch(bytes[] calldata calls, bool revertOnFail)
    external payable
    returns (bool[] memory successes, bytes[] memory results)
```

**Parameters**

| Name         | Type      | Description                                                             |
| ------------ | --------- | ----------------------------------------------------------------------- |
| calls        | `bytes[]` | An array of inputs for each call.                                       |
| revertOnFail | `bool`    | If True then reverts after a failed call and stops doing further calls. |

**Return values**

| Name      | Type      | Description                                                                          |
| --------- | --------- | ------------------------------------------------------------------------------------ |
| successes | `bool[]`  | `An array indicating the success of a call, mapped one-to-one to calls.`             |
| results   | `bytes[]` | `An array with the returned data of each function call, mapped one-to-one to calls.` |

### `batchFlashLoan | nonpayable​​`

```solidity
function batchFlashLoan(
    IBatchFlashBorrower borrower,
    address[] calldata receivers,
    IERC20[] calldata tokens,
    uint256[] calldata amounts,
    bytes calldata data
) public {
```

**Parameters**

| Name      | Type        | Description                                                                                                  |
| --------- | ----------- | ------------------------------------------------------------------------------------------------------------ |
| borrower  | `address`   | `The address of the contract that implements and conforms to IBatchFlashBorrower and handles the flashloan.` |
| receivers | `address[]` | `The address of the contract that implements and conforms to IBatchFlashBorrower and handles the flashloan.` |
| tokens    | `address[]` | The addresses of the tokens.                                                                                 |
| amounts   | `uint256[]` | amounts of the tokens for each receiver.                                                                     |
| data      | `bytes`     | `The calldata to pass to the borrower contract.`                                                             |

**Return values**

No return values for this function.

### `claimOwnership | nonpayable​​`

```solidity
function claimOwnership() public
```

Needs to be called by pendingOwner to claim ownership.

### `deploy | payable​​`

```solidity
function deploy(
    address masterContract,
    bytes calldata data,
    bool useCreate2
) public payable returns (address cloneAddress)
```

Deploys a given master Contract as a clone.

**Parameters**

| Name           | Type      | Description                                                                                 |
| -------------- | --------- | ------------------------------------------------------------------------------------------- |
| masterContract | `address` | The address of the contract to clone.                                                       |
| data           | `bytes`   | `Additional abi encoded calldata that is passed to the new clone via IMasterContract.init.` |
| useCreate2     | `bool`    | `Creates the clone by using the CREATE2 opcode, in this case data will be used as salt.`    |

**Return values**

| Name         | Type      | Description                            |
| ------------ | --------- | -------------------------------------- |
| cloneAddress | `address` | Address of the created clone contract. |

### `deposit | payable​​`

```solidity
function deposit(
    IERC20 token_,
    address from,
    address to,
    uint256 amount,
    uint256 share
) public payable allowed(from) returns (uint256 amountOut, uint256 shareOut)
```

**Parameters**

| Name    | Type      | Description                                                                    |
| ------- | --------- | ------------------------------------------------------------------------------ |
| token\_ | `address` | The ERC-20 token to deposit.                                                   |
| from    | `address` | which account to pull the tokens from.                                         |
| to      | `address` | which account to push the tokens to.                                           |
| amount  | `uint256` | Token amount in native representation to deposit.                              |
| share   | `uint256` | `Token amount represented in shares to deposit. Takes precedence over amount.` |

**Return values**

| Name      | Type      | Text                                        |
| --------- | --------- | ------------------------------------------- |
| amountOut | `uint256` | The amount deposited.                       |
| shareOut  | `uint256` | The deposited amount represented in shares. |

### `flashLoan | nonpayable​​`

```solidity
function flashLoan(
    IFlashBorrower borrower,
    address receiver,
    IERC20 token,
    uint256 amount,
    bytes calldata data
) public
```

**Parameters**

| Name     | Type      | Text                                                                                                    |
| -------- | --------- | ------------------------------------------------------------------------------------------------------- |
| borrower | `address` | `The address of the contract that implements and conforms to IFlashBorrower and handles the flashloan.` |
| receiver | `address` | Address of the token receiver.                                                                          |
| token    | `address` | The address of the token to receive.                                                                    |
| amount   | `uint256` | Amount of the tokens to receive.                                                                        |
| data     | `bytes`   | `The calldata to pass to the borrower contract.`                                                        |

**Return values**

No return values for this function.

### `harvest | nonpayable​​`

```solidity
function harvest(
    IERC20 token,
    bool balance,
    uint256 maxChangeAmount
) public
```

The actual process of yield farming. Executes the strategy of token. Optionally does housekeeping if balance is true.

**Parameters**

| Name            | Type      | Description                                                                        |
| --------------- | --------- | ---------------------------------------------------------------------------------- |
| token           | `address` | The address of the token for which a strategy is deployed.                         |
| balance         | `bool`    | True if housekeeping should be done.                                               |
| maxChangeAmount | `uint256` | `The maximum amount for either pulling or pushing from/to the IStrategy contract.` |

**Return values**

No return values for this function.

### `masterContractApproved | view​​`

```solidity
mapping(address => mapping(address => bool)) public masterContractApproved;
```

masterContract to user to approval state, returns True if the user has approved the masterContract.

**Parameters**

| Name | Type      | Description                    |
| ---- | --------- | ------------------------------ |
| /    | `address` | Address of the masterContract. |
| /    | `address` | Address of the user.           |

**Return values**

| Name | Type   | Description                                             |
| ---- | ------ | ------------------------------------------------------- |
| /    | `bool` | Returns True if the masterContract is approved by user. |

### `masterContractOf | view​​`

```solidity
mapping(address => address) public masterContractOf;
```

Mapping from clone contracts to their masterContract.

**Parameters**

| Name | Type      | Description                     |
| ---- | --------- | ------------------------------- |
| /    | `address` | Address of the cloned contract. |

**Return values**

| Name | Type      | Description                                                  |
| ---- | --------- | ------------------------------------------------------------ |
| /    | `address` | Address of the masterContract clone contract is cloned from. |

### `nonces | view​​`

```solidity
mapping(address => uint256) public nonces;
```

User nonces for masterContract approvals.

**Parameters**

| Name | Type      | Description   |
| ---- | --------- | ------------- |
| /    | `address` | User address. |

**Return values**

| Name | Type      | Description        |
| ---- | --------- | ------------------ |
| /    | `uint256` | Nonce of the user. |

#### `owner | view​​`

Owner of the DegenBox contract. Can perform actions that onlyOwner() can perform, such as strategy updates.

**Parameters**

No parameters for this function.

**Return values**

| Name | Type      | Description          |
| ---- | --------- | -------------------- |
| /    | `address` | Address of the Owner |

### `pendingOwner | view​​`

When transferring ownership, the future owner is pending until they claims ownership.

**Return values**

| Name | Type      | Description                   |
| ---- | --------- | ----------------------------- |
| /    | `address` | Address of the pending owner. |

### `pendingStrategy | view​​`

Strategy change being submitted to a delay of 3 days, the setStrategy function is called twice, once to declare a strategy change, and another time after STRATEGY\_DELAY (3 days). In between those calls, the new strategy is pendingStrategy.

**Parameters**

| Name | Type      | Description                                                 |
| ---- | --------- | ----------------------------------------------------------- |
| /    | `address` | The address of the token that maps to a strategy to change. |

**Return values**

| Name | Type      | Description                                               |
| ---- | --------- | --------------------------------------------------------- |
| /    | `address` | `The address of the contract that conforms to IStrategy.` |

### `permitToken | nonpayable​​`

```solidity
function permitToken(
        IERC20 token,
        address from,
        address to,
        uint256 amount,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) public
```

**Parameters**

| Name     | Type      | Text                                                      |
| -------- | --------- | --------------------------------------------------------- |
| token    | `address` | The ERC20 token that supports permit to call permit on.   |
| from     | `address` | The token owner's address                                 |
| to       | `address` | The token spender's address                               |
| amount   | `uint256` | The amount to be approved.                                |
| deadline | `uint256` | Timestamp in the future until which permit can be called. |
| v        | `uint8`   | v of the ECDSA signature.                                 |
| r        | `bytes32` | r of the ECDSA signature.                                 |
| s        | `bytes32` | s of the ECDSA signature.                                 |

**Return values** No return values for this function.

### `registerProtocol | nonpayable​​`

**Parameters** No parameters for this function. **Return values** No return values for this function.

### `setMasterContractApproval | nonpayable​​`

**Parameters**

| Name           | Type      |
| -------------- | --------- |
| user           | `address` |
| masterContract | `address` |
| approved       | `bool`    |
| v              | `uint8`   |
| r              | `bytes32` |
| s              | `bytes32` |

**Return values** No return values for this function.

### `setStrategy | nonpayable​​`

```solidity
function setStrategy(IERC20 token, IStrategy newStrategy) public onlyOwner
```

Sets the contract address of a new strategy that conforms to IStrategy for token. Must be called twice with the same arguments. A new strategy becomes pending first and can be activated once STRATEGY\_DELAY is over. In the case of the Degenbox, STRATEGY\_DELAY is 3 days.

**Parameters**

| Name        | Type      | Description                                                 |
| ----------- | --------- | ----------------------------------------------------------- |
| token       | `address` | The address of the token that maps to a strategy to change. |
| newStrategy | `address` | `The address of the contract that conforms to IStrategy.`   |

**Return values** No return values for this function.

### `setStrategyTargetPercentage | nonpayable​​`

```solidity
function setStrategyTargetPercentage(
         IERC20 token,
         uint64 targetPercentage_
      ) public onlyOwner {
```

Sets the target percentage of the strategy for token. It corresponds to the percentage of tokens deployed to the Strategy, vs kept in DegenBox.

**Parameters**

| Name               | Type      | Description                                                                    |
| ------------------ | --------- | ------------------------------------------------------------------------------ |
| token              | `address` | The address of the token that maps to a strategy to change.                    |
| targetPercentage\_ | `uint64`  | `The new target in percent. Must be lesser or equal to MAX_TARGET_PERCENTAGE.` |

**Return values** No return values for this function.

### `strategy | view​​`

Returns the address of the Strategy mapped to the given ERC20 token.

**Parameters**

| Name | Type      | Description                                      |
| ---- | --------- | ------------------------------------------------ |
| /    | `address` | The address of the token that maps to a strategy |

**Return values**

| Name | Type      | Text                                                                          |
| ---- | --------- | ----------------------------------------------------------------------------- |
| /    | `address` | `The address of the current strategy: a contract that conforms to IStrategy.` |

### `strategyData | view​​`

**Parameters**

| Name | Type      | Description                                      |
| ---- | --------- | ------------------------------------------------ |
| /    | `address` | The address of the token that maps to a strategy |

**Return values**

| Name              | Type      | Description                                                           |
| ----------------- | --------- | --------------------------------------------------------------------- |
| strategyStartDate | `uint64`  | Timestamp of the strategy start date.                                 |
| targetPercentage  | `uint64`  | Current targetPercentage (% of funds wanted in strategy) of the token |
| balance           | `uint128` | Current balance held by strategy (before harvest)                     |

### `toAmount | view​​`

```solidity
function toAmount(
    IERC20 token,
    uint256 share,
    bool roundUp
) external view returns (uint256 amount)
```

Converts amounts of shares of a token into it's native representation.

**Parameters**

| Name    | Type      | Description                         |
| ------- | --------- | ----------------------------------- |
| token   | `address` | The ERC-20 token.                   |
| share   | `uint256` | The amount of shares.               |
| roundUp | `bool`    | If the result should be rounded up. |

**Return values**

| Name   | Type      | Description                                       |
| ------ | --------- | ------------------------------------------------- |
| amount | `uint256` | The share amount back into native representation. |

### `toShare | view​​`

```solidity
function toShare(
    IERC20 token,
    uint256 amount,
    bool roundUp
) external view returns (uint256 share)
```

Converts amounts of a token in it's native representation into shares.

**Parameters**

| Name    | Type      | Description                                 |
| ------- | --------- | ------------------------------------------- |
| token   | `address` | The ERC-20 token.                           |
| amount  | `uint256` | `The token amount.`                         |
| roundUp | `bool`    | `If the result share should be rounded up.` |

**Return values**

| Name  | Type      | Description                             |
| ----- | --------- | --------------------------------------- |
| share | `uint256` | The token amount represented in shares. |

### `totals | view​​`

```solidity
mapping(IERC20 => Rebase) public totals;
```

Keeps track of how many tokens are stored by DegenBox as well as how many shares are held by users.

**Parameters**

| Name | Type      | Description                        |
| ---- | --------- | ---------------------------------- |
| /    | `address` | ERC20 Address which total we want. |

**Return values**

| Name    | Type      | Text                                                                    |
| ------- | --------- | ----------------------------------------------------------------------- |
| elastic | `uint128` | How many tokens (in native representation) are deposited into DegenBox. |
| base    | `uint128` | How many shares of tokens are held by users.                            |

### `transfer | nonpayable​​`

```solidity
function transfer(
    IERC20 token,
    address from,
    address to,
    uint256 share
) public allowed(from)
```

Transfer shares from a user account to another one.

**Parameters**

| Name  | Type      | Text                                  |
| ----- | --------- | ------------------------------------- |
| token | `address` | The ERC20 token to transfer.          |
| from  | `address` | `The account to transfer token from.` |
| to    | `address` | `The account to transfer token to.`   |
| share | `uint256` | The amount of shares to transfer.     |

**Return values** No return values for this function.

### `transferMultiple | nonpayable​​`

```solidity
function transferMultiple(
    IERC20 token,
    address from,
    address[] calldata tos,
    uint256[] calldata shares
) public allowed(from) {
```

Transfer shares from a user account to multiple other ones.

**Parameters**

| Name   | Type        | Text                                                      |
| ------ | ----------- | --------------------------------------------------------- |
| token  | `address`   | The ERC20 token to transfer.                              |
| from   | `address`   | `The account to transfer token from.`                     |
| tos    | `address[]` | `The accounts to transfer token to.`                      |
| shares | `uint256[]` | `The amount of token in shares for each receiver in tos.` |

**Return values** No return values for this function.

### `transferOwnership | nonpayable​​`

```solidity
function transferOwnership(
    address newOwner,
    bool direct,
    bool renounce
) public onlyOwner
```

Transfers ownership to newOwner. Either directly or claimable by the new pending owner. Can only be invoked by the current owner.

**Parameters**

| Name     | Type      | Description                                                                                     |
| -------- | --------- | ----------------------------------------------------------------------------------------------- |
| newOwner | `address` | Address of the new owner.                                                                       |
| direct   | `bool`    | `True if newOwner should be set immediately. False if newOwner needs to use claimOwnership.`    |
| renounce | `bool`    | `Allows the newOwner to be address(0) if direct and renounce is True. Has no effect otherwise.` |

**Return values** No return values for this function.

### `whitelistMasterContract | nonpayable​​`

```solidity
function whitelistMasterContract(
    address masterContract,
    bool approved
) public onlyOwnersoli
```

Enables or disables a contract for approval without signed message.

**Parameters**

| Name           | Type      | Description                                   |
| -------------- | --------- | --------------------------------------------- |
| masterContract | `address` | masterContract which status is being changed. |
| approved       | `bool`    | New approval status.                          |

**Return values** No return values for this function.

### `whitelistedMasterContracts | view​​`

Returns True if a masterContract is whitelisted.

**Parameters**

| Name | Type      | Description                    |
| ---- | --------- | ------------------------------ |
| /    | `address` | Address of the masterContract. |

**Return values**

| Name | Type   | Description          |
| ---- | ------ | -------------------- |
| /    | `bool` | Whitelisting status. |

### `withdraw | nonpayable​​`

```solidity
function withdraw(
    IERC20 token_,
    address from,
    address to,
    uint256 amount,
    uint256 share
) public allowed(from) returns (uint256 amountOut, uint256 shareOut)
```

Withdraws an amount of token from a user account.

**Parameters**

| Name    | Type      | Description                                                             |
| ------- | --------- | ----------------------------------------------------------------------- |
| token\_ | `address` | The ERC-20 token to withdraw.                                           |
| from    | `address` | From which user to pull the tokens.                                     |
| to      | `address` | To which user to push the tokens.                                       |
| amount  | `uint256` | `Amount of tokens. Either one of amount or share needs to be supplied.` |
| share   | `uint256` | `Like above, but share takes precedence over amount.`                   |

**Return values**

| Name      | Type      | Description                                          |
| --------- | --------- | ---------------------------------------------------- |
| amountOut | `uint256` | Amount of token withdrawn, in native representation. |
| shareOut  | `uint256` | Shares of token withdrawn.                           |

## Events (for EVM logging)​​

* ​LogDeploy​
* ​LogDeposit​
* ​LogFlashLoan​
* ​LogRegisterProtocol​
* ​LogSetMasterContractApproval​
* ​LogStrategyDivest​
* ​LogStrategyInvest​
* ​LogStrategyLoss​
* ​LogStrategyProfit​
* ​LogStrategyQueued​
* ​LogStrategySet​
* ​LogStrategyTargetPercentage​
* ​LogTransfer​
* ​LogWhiteListMasterContract​
* ​LogWithdraw​
* ​OwnershipTransferred​

​

### `LogDeploy​​`

**Parameters**

| Name           | Type      | Indexed |
| -------------- | --------- | ------- |
| masterContract | `address` | `true`  |
| data           | `bytes`   | `false` |
| cloneAddress   | `address` | `true`  |

### `LogDeposit​​`

**Parameters**

| Name   | Type      | Indexed |
| ------ | --------- | ------- |
| token  | `address` | `true`  |
| from   | `address` | `true`  |
| to     | `address` | `true`  |
| amount | `uint256` | `false` |
| share  | `uint256` | `false` |

### `LogFlashLoan​​`

**Parameters**

| Name      | Type      | Indexed |
| --------- | --------- | ------- |
| borrower  | `address` | `true`  |
| token     | `address` | `true`  |
| amount    | `uint256` | `false` |
| feeAmount | `uint256` | `false` |
| receiver  | `address` | `true`  |

### `LogRegisterProtocol​​`

**Parameters**

| Name     | Type      | Indexed |
| -------- | --------- | ------- |
| protocol | `address` | `true`  |

### `LogSetMasterContractApproval​​`

**Parameters**

| Name           | Type      | Indexed |
| -------------- | --------- | ------- |
| masterContract | `address` | `true`  |
| user           | `address` | `true`  |
| approved       | `bool`    | `false` |

### `LogStrategyDivest​​`

**Parameters**

| Name   | Type      | Indexed |
| ------ | --------- | ------- |
| token  | `address` | `true`  |
| amount | `uint256` | `false` |

### `LogStrategyInvest​​`

**Parameters**

| Name   | Type      | Indexed |
| ------ | --------- | ------- |
| token  | `address` | `true`  |
| amount | `uint256` | `false` |

### `LogStrategyLoss​​`

**Parameters**

| Name   | Type      | Indexed |
| ------ | --------- | ------- |
| token  | `address` | `true`  |
| amount | `uint256` | `false` |

### `LogStrategyProfit​​`

**Parameters**

| Name   | Type      | Indexed |
| ------ | --------- | ------- |
| token  | `address` | `true`  |
| amount | `uint256` | `false` |

### `LogStrategyQueued​​`

**Parameters**

| Name     | Type      | Indexed |
| -------- | --------- | ------- |
| token    | `address` | `true`  |
| strategy | `address` | `true`  |

### `LogStrategySet​​`

**Parameters**

| Name     | Type      | Indexed |
| -------- | --------- | ------- |
| token    | `address` | `true`  |
| strategy | `address` | `true`  |

### `LogStrategyTargetPercentage​​`

**Parameters**

| Name             | Type      | Indexed |
| ---------------- | --------- | ------- |
| token            | `address` | `true`  |
| targetPercentage | `uint256` | `false` |

### `LogTransfer​​`

**Parameters**

| Name  | Type      | Indexed |
| ----- | --------- | ------- |
| token | `address` | `true`  |
| from  | `address` | `true`  |
| to    | `address` | `true`  |
| share | `uint256` | `false` |

### `LogWhiteListMasterContract​​`

**Parameters**

| Name           | Type      | Indexed |
| -------------- | --------- | ------- |
| masterContract | `address` | `true`  |
| approved       | `bool`    | `false` |

### `LogWithdraw​​`

**Parameters**

| Name   | Type      | Indexed |
| ------ | --------- | ------- |
| token  | `address` | `true`  |
| from   | `address` | `true`  |
| to     | `address` | `true`  |
| amount | `uint256` | `false` |
| share  | `uint256` | `false` |

### `OwnershipTransferred​​`

**Parameters**

| Name             | Type      | Indexed |
| ---------------- | --------- | ------- |
| clopreviousOwner | `address` | `true`  |
| newOwner         | `address` | `true`  |
