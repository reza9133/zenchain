# zenchain
its about zenchain project
pragma solidity ^0.8.0;

/** @dev The contract's address. */
address constant FAST_UNSTAKE_ADDRESS = 0x0000000000000000000000000000000000000801;

/** @dev The contract's instance. */
NativeFastUnstake constant FAST_UNSTAKE_CONTRACT = NativeFastUnstake(FAST_UNSTAKE_ADDRESS);

interface NativeFastUnstake {
    /**
    * @notice Register oneself for fast-unstake. The stash must have no ongoing unlocking chunks. If successful, this will fully unbond and chill the stash. Then, it will enqueue the stash to be checked in further blocks.
    * If by the time this is called, the stash is actually eligible for fast-unstake, then they are guaranteed to remain eligible, because the call will chill them as well.
    * If the check works, the entire staking data is removed, i.e. the stash is fully unstaked.
    * If the check fails, the stash remains chilled and waiting for being unbonded as in with the normal staking system, but they lose part of their unbonding chunks due to consuming the chain’s resources.
    */
    function registerFastUnstake() external;

    /**
    * @notice Deregister oneself from the fast-unstake.
    * This is useful if one is registered, they are still waiting, and they change their mind.
    * Note that the associated stash is still fully unbonded and chilled as a consequence of calling Pallet::register_fast_unstake. Therefore, this should probably be followed by a call to rebond in the staking system.
    */
    function deregister() external;

    /**
    * @dev Populates the precompile address with dummy bytecode, allowing the precompile to pass related checks.
    * For example, the presence of bytecode allows wallets like Metamask to recognize the precompile as a contract.
    */
    function populateBytecode() external;
}
