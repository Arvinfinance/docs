# MasterChef

The MasterChef is the reward distributor.

For now, we have 5 types of the reward distribution

* Stake VIN, reward IN (from 50% of the protocols income)
* Stake ARV, reward IN (from 20% of the treasury income)
* Stake ( IN / ? )LP, reward VIN (400 VIN per day, distribute 180 days)R
* Lock VIN, reward ARV and IN (the IN reward is shared with the Stake VIN pool, the ARV reward is base on [the ARV distribution rule](about:blank). )
* Borrow IN, reward VIN (see [the VIN Boost rule](about:blank))

### Constructor(address,address,address,address,uint256)

| Parameter      | Description                           |
| -------------- | ------------------------------------- |
| \_arv          | The address of the ARV                |
| \_vin          | The address of VIN                    |
| \_in           | The address of IN                     |
| \_lp           | The address of LP                     |
| startTimestamp | The timestamp of the reward timestamp |

## Pool Ids

```solidity
uint256 public constant LOCK_POOL = 0;//Lock VIN, reward ARV
uint256 public constant VIN_POOL = 1;//Stake/Lock VIN, reward IN
uint256 public constant ARV_POOL = 2;//Stake ARV, reward IN
uint256 public constant LP_POOL = 3;//Stake ( IN / ? )LP, reward VIN
```

## Write Functions

### deposit

deposit VIN/ARV/LP to the pool.

<pre class="language-solidity"><code class="lang-solidity"><strong>function deposit(uint256 _pid, uint256 _amount) external;
</strong></code></pre>

**Parameters**

| Name     | Type    | Description                                                                           |
| -------- | ------- | ------------------------------------------------------------------------------------- |
| \_pid    | uint256 | the id of the staking pool. see Pool Ids                                              |
| \_amount | uint256 | the token amount that you want to deposit,you should approve to the masterChef first. |

### depositLock

deposit VIN to the lock pool, VIN will be lock for 21 days.

```solidity
function depositLock(uint256 _amount) external;
```

**Parameters**

| Name     | Type    | Description                                                                        |
| -------- | ------- | ---------------------------------------------------------------------------------- |
| \_amount | uint256 | the token amount that you want to lock,you should approve to the masterChef first. |

### withdraw

withdraw VIN/ARV/LP from the pool.

```solidity
function withdraw(uint256 _pid, uint256 _amount) external;
```

**Parameters**

| Name     | Type    | Description                                |
| -------- | ------- | ------------------------------------------ |
| \_pid    | uint256 | the id of the staking pool. see Pool Ids   |
| \_amount | uint256 | the token amount that you want to withdraw |

### withdrawLock

withdraw the locked VIN from the lock pool. see getUnlockableAmount to get the unlockable amount.

```solidity
function withdrawLock(uint256 _amount) external;
```

**Parameters**

| Name     | Type    | Description                               |
| -------- | ------- | ----------------------------------------- |
| \_amount | uint256 | the token amount that you want to unlock. |

### claimPending

claim pending rewards of the pool, if the pool is the VIN Boost pool, the rewards need to vest by calling the function vestingPendingReward

```solidity
function claimPending(uint256 _pid) external;
```

### vestingPendingReward

vesting the VIN Boost reward, it will be vesting for 21 days. after vesting, call the function claimVestingReward to claim rewards

```solidity
function vestingPendingReward(bool claim) external;
```

### claimVestingReward

claim all claimable VIN Boost rewards

```solidity
function claimVestingReward() public
```

### updateRewardPerBlock

update the VIN Boost reward of the pool

```solidity
function updateRewardPerBlock(uint256 _rewardPerSecond, uint256 _pid) public onlyOwner
```

### add

add a VIN Boost pool

```solidity
function add(
    uint256 _rewardPerSecond, 
    address _cauldronAddress, 
    uint256 startTimestamp, 
    bool _withUpdate
) public onlyOwner
```

### addRewardToPool

add rewards to the ARV/VIN staking pool, basicly only the Cauldron will call this function. but if someone is charitable, it can let them to donate to the ARV/VIN staking pool.

```solidity
function addRewardToPool(uint256 poolId, uint256 amount) public
```

### emergencyWithdraw

emergency withdraw the token without any rewards

```solidity
function emergencyWithdraw(uint256 _pid) external;
```

## **View Functions**

### getUnlockableAmount

get user's unlockable VIN amount

```solidity
function getUnlockableAmount(address user) external view returns (uint256 amount);
```

**Parameters**

| Name | Type    | Description    |
| ---- | ------- | -------------- |
| user | address | user's address |

**Return Values**

| Name   | Type    | Description                  |
| ------ | ------- | ---------------------------- |
| amount | uint256 | user's unlockable VIN amount |

### getLockAmount

get user's total locked VIN amount

```solidity
function getLockAmount(address user) external view returns (uint256 amount);
```

**Parameters**

| Name | Type    | Description    |
| ---- | ------- | -------------- |
| user | address | user's address |

**Return Values**

| Name   | Type    | Description              |
| ------ | ------- | ------------------------ |
| amount | uint256 | user's locked VIN amount |

### getLockInfo

get user's VIN lock info

```solidity
function getLockInfo(address user) external view returns (LockDetail[] memory locks);
```

```solidity
struct LockDetail {
        uint256 lockAmount;//the amount of locked VIN
        uint256 unlockAmount;//the amount of unlocked VIN
        uint256 unlockTimestamp;//the timesatmp that can unlock VIN
    }
```

**Parameters**

| Name | Type    | Description    |
| ---- | ------- | -------------- |
| user | address | user's address |

**Return Values**

| Name  | Type          | Description          |
| ----- | ------------- | -------------------- |
| locks | LockDetail\[] | user's VIN lock list |

### userVestingInfo

get user vesting VIN info

```solidity
mapping(address => VestingInfo[]) public userVestingInfo;
```

<pre class="language-solidity"><code class="lang-solidity"><strong>struct VestingInfo {
</strong>    uint256 vestingReward;//vesting VIN amount
    uint256 claimTime;//when can claim the vesting reward
    bool isClaimed;
}
</code></pre>

**Parameters**

| Name | Type    | Description    |
| ---- | ------- | -------------- |
| user | address | user's address |

**Return Values**

| Name    | Type           | Description             |
| ------- | -------------- | ----------------------- |
| vesting | VestingInfo\[] | user's vesting VIN list |

### pendingReward

get the amount of user's pending rewards

```solidity
function pendingReward(uint256 _pid, address _user) external view returns (uint256);
```

### userPendingReward

get the amount of user's rewards that is not vesting

```solidity
function userPendingReward(address user) external returns (uint256 pendingReward);
```

### totalStake

get the amount of pool's stake token

```solidity
function totalStake(uint256 _pid) external returns (uint256 stakeAmount);
```

### estimateARVCirculatingSupply

estimate the ARV circulating supply, include the amount that is not claimed by user.

```solidity
function estimateARVCirculatingSupply() public view returns (uint256 circulatingSupply);
```

### cauldronPoolInfo

get the pool id of cauldron

```solidity
function cauldronPoolInfo(address cauldron) external view returns (uint256);
```
