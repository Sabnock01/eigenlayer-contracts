# IPausable
[Git Source](https://github.com/Sabnock01/eigenlayer-contracts/blob/fa80db0202cf74fb2bae3ffc6aa6db988074a698/src/contracts/interfaces/IPausable.sol)

**Author:**
Layr Labs, Inc.

Contracts that inherit from this contract may define their own `pause` and `unpause` (and/or related) functions.
These functions should be permissioned as "onlyPauser" which defers to a `PauserRegistry` for determining access control.

*Pausability is implemented using a uint256, which allows up to 256 different single bit-flags; each bit can potentially pause different functionality.
Inspiration for this was taken from the NearBridge design here https://etherscan.io/address/0x3FEFc5A4B1c02f21cBc8D3613643ba0635b9a873#code.
For the `pause` and `unpause` functions we've implemented, if you pause, you can only flip (any number of) switches to on/1 (aka "paused"), and if you unpause,
you can only flip (any number of) switches to off/0 (aka "paused").
If you want a pauseXYZ function that just flips a single bit / "pausing flag", it will:
1) 'bit-wise and' (aka `&`) a flag with the current paused state (as a uint256)
2) update the paused state to this new value*

*We note as well that we have chosen to identify flags by their *bit index* as opposed to their numerical value, so, e.g. defining `DEPOSITS_PAUSED = 3`
indicates specifically that if the *third bit* of `_paused` is flipped -- i.e. it is a '1' -- then deposits should be paused*


## Functions
### pauserRegistry

Address of the `PauserRegistry` contract that this contract defers to for determining access control (for pausing).


```solidity
function pauserRegistry() external view returns (IPauserRegistry);
```

### pause

This function is used to pause an EigenLayer contract's functionality.
It is permissioned to the `pauser` address, which is expected to be a low threshold multisig.

*This function can only pause functionality, and thus cannot 'unflip' any bit in `_paused` from 1 to 0.*


```solidity
function pause(uint256 newPausedStatus) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newPausedStatus`|`uint256`|represents the new value for `_paused` to take, which means it may flip several bits at once.|


### pauseAll

Alias for `pause(type(uint256).max)`.


```solidity
function pauseAll() external;
```

### unpause

This function is used to unpause an EigenLayer contract's functionality.
It is permissioned to the `unpauser` address, which is expected to be a high threshold multisig or governance contract.

*This function can only unpause functionality, and thus cannot 'flip' any bit in `_paused` from 0 to 1.*


```solidity
function unpause(uint256 newPausedStatus) external;
```
**Parameters**

|Name|Type|Description|
|----|----|-----------|
|`newPausedStatus`|`uint256`|represents the new value for `_paused` to take, which means it may flip several bits at once.|


### paused

Returns the current paused status as a uint256.


```solidity
function paused() external view returns (uint256);
```

### paused

Returns 'true' if the `indexed`th bit of `_paused` is 1, and 'false' otherwise


```solidity
function paused(uint8 index) external view returns (bool);
```
