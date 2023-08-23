// SPDX-License-Identifier: MIT

// Original license: SPDX_License_Identifier: None
pragma solidity ^0.8.0;

/// @title Interface that describes the functions that the BSCBridge contract should implement
/// @author SteffQing
/// @notice Explain to an end user what this does
/// @dev Explain to a developer any extra details
interface IBridgeBase {
    /// @notice event that is emitted when a user deposits tokens on the BSC chain
    /// @dev event used to trigger the script to release tokens on the Tron chain
    /// @param from The address of the user who wants to deposit
    /// @param to The address of the user to receive the deposit on the Tron chain
    /// @param value The amount of tokens to be deposited
    event Deposit(address indexed from, string to, uint256 value);

    /// @notice event that is emitted when a user withdraws tokens on the BSC chain
    /// @dev event used to trigger the script to send x tokens on BSC to user
    /// @param to The address of the user who wants to withdraw
    /// @param value The amount of tokens to be withdrawn
    event Withdraw(string from, address indexed to, uint256 value);

    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Burn(address indexed from, uint256 value);

    /// @notice function to handle user's deposit which locks the token on the BSC chain
    /// @dev uses strings too denote address to receive deposit on the Tron chain
    /// @param to The address of the user who wants to deposit
    /// @param value The amount of tokens to be deposited
    function deposit(string calldata to, uint256 value) external;

    /// @notice function to handle user's withdrawal controlled by the bridge
    /// @param to The address of the user who wants to withdraw
    /// @param value The amount of tokens to be withdrawn
    function withdraw(string calldata from, address to, uint256 value) external;

    /// @notice function to burn tokens after deposit is confirmed on BSC
    /// @dev function is called by the bridge after deposit is confirmed on BSC
    /// @param from The address of the user depositing
    /// @param value The amount of tokens to be burnt
    function burn(address from, uint256 value) external;

    /// @notice function to check balance of bridge
    /// @return uint256 balance of bridge
    function bridgeBalance() external returns (uint);

    /// @notice Get information about the bridge contract
    /// @return crossBridgeAddress of the contract on the cross chain,
    /// @return minDepositAmount The minimum deposit amount required for bridging
    /// @return bridgeFees The fee amount required for bridging
    /// @return bridgeClosed The current state of the bridge
    function getBridgeInfo()
        external
        view
        returns (
            string memory crossBridgeAddress,
            uint256 minDepositAmount,
            uint256 bridgeFees,
            bool bridgeClosed
        );

    /**
     * @notice Check the status of a user's pending deposit
     * @param user The address of the user
     * @return hasPendingDeposit Whether the user has a pending deposit
     */
    function getDepositStatus(
        address user
    ) external view returns (bool hasPendingDeposit);

    /**
     * @notice Cancel a user's pending withdrawal and release locked tokens
     */
    function cancelPendingDeposit() external;

    /**
     * @notice Set the minimum deposit amount required for bridging
     * @param amount The new minimum deposit amount
     */
    function setMinDepositAmount(uint256 amount) external;

    /**
     * @notice Set the fee amount required for bridging
     * @param amount The new fee amount
     */
    function setBridgeFees(uint256 amount) external;

    /**
     * @notice Pause or unpause the bridge functionality in emergencies
     * @param paused Whether to pause (true) or unpause (false) the bridge
     */
    function emergencyPause(bool paused) external;

    /**
     * @notice Set a new bridge contract address (for upgrades, if needed)
     * @param newBridgeAddress The address of the new bridge contract
     */
    function setCrossBridgeContract(string calldata newBridgeAddress) external;

    /// @notice claim bridge fees callable by owner
    function claimFees() external;

    /// @notice set bridge admin callable by owner
    function setBridgeAdmin(address newAdmin) external;

    /// @notice add whitelisted address callable by owner
    function addWhitelistedAddress(address whitelist_) external;

    /// @notice remove whitelisted address callable by owner
    function removeWhitelistedAddress(address whitelist_) external;

    /// @notice set new bucket capacity for whitelist address callable by owner
    function setBucketCapacity(address whitelist_, uint256 capacity_) external;

    /// @notice add liquidity to bridge
    function addLiquidity(uint256 amount) external;

    /// @notice remove liquidity from bridge
    function removeLiquidity(uint256 amount) external;

    /// @notice get bridge liquidity and generated fees
    function getBridgeLiquidityAndFees()
        external
        view
        returns (uint256, uint256);
}


// File interface/IERC20.sol

// Original license: SPDX_License_Identifier: AGPL-3.0
pragma solidity ^0.8.0;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Returns the amount of tokens that will be burned on transfer.
     */
    function burnPercentage() external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `recipient`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}


// File interface/SafeERC20.sol

// Original license: SPDX_License_Identifier: None
pragma solidity ^0.8.0;
/// @title Gnosis Protocol v2 Safe ERC20 Transfer Library
/// @author Gnosis Developers
/// @dev Gas-efficient version of Openzeppelin's SafeERC20 contract.
library GPv2SafeERC20 {
    /// @dev Wrapper around a call to the ERC20 function `transfer` that reverts
    /// also when the token returns `false`.
    function safeTransfer(IERC20 token, address to, uint256 value) internal {
        bytes4 selector_ = token.transfer.selector;

        // solhint-disable-next-line no-inline-assembly
        assembly {
            let freeMemoryPointer := mload(0x40)
            mstore(freeMemoryPointer, selector_)
            mstore(
                add(freeMemoryPointer, 4),
                and(to, 0xffffffffffffffffffffffffffffffffffffffff)
            )
            mstore(add(freeMemoryPointer, 36), value)

            if iszero(call(gas(), token, 0, freeMemoryPointer, 68, 0, 0)) {
                returndatacopy(0, 0, returndatasize())
                revert(0, returndatasize())
            }
        }

        require(getLastTransferResult(token), "GPv2: failed transfer");
    }

    /// @dev Wrapper around a call to the ERC20 function `transferFrom` that
    /// reverts also when the token returns `false`.
    function safeTransferFrom(
        IERC20 token,
        address from,
        address to,
        uint256 value
    ) internal {
        bytes4 selector_ = token.transferFrom.selector;

        // solhint-disable-next-line no-inline-assembly
        assembly {
            let freeMemoryPointer := mload(0x40)
            mstore(freeMemoryPointer, selector_)
            mstore(
                add(freeMemoryPointer, 4),
                and(from, 0xffffffffffffffffffffffffffffffffffffffff)
            )
            mstore(
                add(freeMemoryPointer, 36),
                and(to, 0xffffffffffffffffffffffffffffffffffffffff)
            )
            mstore(add(freeMemoryPointer, 68), value)

            if iszero(call(gas(), token, 0, freeMemoryPointer, 100, 0, 0)) {
                returndatacopy(0, 0, returndatasize())
                revert(0, returndatasize())
            }
        }

        require(getLastTransferResult(token), "GPv2: failed transferFrom");
    }

    /// @dev Verifies that the last return was a successful `transfer*` call.
    /// This is done by checking that the return data is either empty, or
    /// is a valid ABI encoded boolean.
    function getLastTransferResult(
        IERC20 token
    ) private view returns (bool success) {
        // NOTE: Inspecting previous return data requires assembly. Note that
        // we write the return data to memory 0 in the case where the return
        // data size is 32, this is OK since the first 64 bytes of memory are
        // reserved by Solidy as a scratch space that can be used within
        // assembly blocks.
        // <https://docs.soliditylang.org/en/v0.7.6/internals/layout_in_memory.html>
        // solhint-disable-next-line no-inline-assembly
        assembly {
            /// @dev Revert with an ABI encoded Solidity error with a message
            /// that fits into 32-bytes.
            ///
            /// An ABI encoded Solidity error has the following memory layout:
            ///
            /// ------------+----------------------------------
            ///  byte range | value
            /// ------------+----------------------------------
            ///  0x00..0x04 |        selector("Error(string)")
            ///  0x04..0x24 |      string offset (always 0x20)
            ///  0x24..0x44 |                    string length
            ///  0x44..0x64 | string value, padded to 32-bytes
            function revertWithMessage(length, message) {
                mstore(0x00, "\x08\xc3\x79\xa0")
                mstore(0x04, 0x20)
                mstore(0x24, length)
                mstore(0x44, message)
                revert(0x00, 0x64)
            }

            switch returndatasize()
            // Non-standard ERC20 transfer without return.
            case 0 {
                // NOTE: When the return data size is 0, verify that there
                // is code at the address. This is done in order to maintain
                // compatibility with Solidity calling conventions.
                // <https://docs.soliditylang.org/en/v0.7.6/control-structures.html#external-function-calls>
                if iszero(extcodesize(token)) {
                    revertWithMessage(20, "GPv2: not a contract")
                }

                success := 1
            }
            // Standard ERC20 transfer returning boolean success value.
            case 32 {
                returndatacopy(0, 0, returndatasize())

                // NOTE: For ABI encoding v1, any non-zero value is accepted
                // as `true` for a boolean. In order to stay compatible with
                // OpenZeppelin's `SafeERC20` library which is known to work
                // with the existing ERC20 implementation we care about,
                // make sure we return success for any non-zero return value
                // from the `transfer*` call.
                success := iszero(iszero(mload(0)))
            }
            default {
                revertWithMessage(31, "GPv2: malformed transfer result")
            }
        }
    }
}


// File contracts/BSCBridge.sol

// Original license: SPDX_License_Identifier: None
pragma solidity ^0.8.0;

/// @title Bridge Contract
/// @author SteffQing
/// @notice This smart contract collects and locks user's token deposit, and emits an event to mint the same amount of token on the cross chain.
/// @dev Inherits from the IBridgeBase interface.
contract Bridge is IBridgeBase {
    using GPv2SafeERC20 for IERC20;
    IERC20 private immutable _token;

    address public bridgeAdmin;
    uint256 private bridgeLiquidity;
    string private _crossBridgeAddress;
    uint256 private _bridgeFee;
    uint256 private _minDepositAmount;
    uint256 private _generatedFees;
    uint256 private _crossChainAddrLength;
    uint256 private constant TIME_INTERVAL = 1 days;
    bool private _paused;

    struct UserDeposit {
        uint256 timestamp;
        uint256 amount;
    }

    struct BucketCapacity {
        uint256 maxBucketCapacity;
        uint256 bucketLevel;
        uint256 lastUpdateTimestamp;
    }

    mapping(address => UserDeposit) public userDeposit;
    mapping(address => BucketCapacity) private isWhitelist;

    constructor(address token_, address whitelist_) {
        _token = IERC20(token_);
        bridgeAdmin = msg.sender;
        _crossChainAddrLength = block.chainid == 97 ? 34 : 42;
        isWhitelist[whitelist_] = BucketCapacity(
            100000 * (10 ** 18),
            0,
            block.timestamp
        );
    }

    modifier onlyWhitelist() {
        BucketCapacity memory bucket = isWhitelist[msg.sender];
        require(bucket.maxBucketCapacity >= 0, "Bridge: Not whitelist");
        _;
    }

    modifier onlyBridgeAdmin() {
        require(
            msg.sender == bridgeAdmin,
            "Bridge: caller is not bridge admin"
        );
        _;
    }

    modifier notPaused() {
        require(!_paused, "Bridge: paused");
        _;
    }

    /// @inheritdoc IBridgeBase
    function deposit(
        string calldata to,
        uint256 amount
    ) external override notPaused {
        require(amount >= _minDepositAmount, "Bridge: amount too small");
        require(
            isAddressLengthEqualTo(to, _crossChainAddrLength),
            "Bridge: to address length invalid"
        );
        require(getDepositStatus(msg.sender), "Bridge: cancelPendingDeposit");
        uint256 transferrableAmount = calculateBurnFee(amount);
        _token.safeTransferFrom(msg.sender, address(this), amount);

        UserDeposit storage userDep = userDeposit[msg.sender];
        userDep.amount = transferrableAmount;
        userDep.timestamp = block.timestamp + 10 minutes;

        _generatedFees += _bridgeFee;
        emit Deposit(msg.sender, to, transferrableAmount);
    }

    /// @inheritdoc IBridgeBase
    function withdraw(
        string calldata from,
        address to,
        uint256 amount
    ) external override onlyWhitelist notPaused {
        require(amount > 0, "Bridge: amount must be greater than 0");
        uint balance = bridgeBalance();
        require(balance >= amount, "Bridge: insufficient balance");
        _updateBucket(amount);
        uint _amount = amount - _bridgeFee;
        uint transferrableAmount = calculateBurnFee(_amount);
        _token.safeTransfer(to, _amount);
        emit Withdraw(from, to, transferrableAmount);
    }

    /// @inheritdoc IBridgeBase
    function burn(
        address from,
        uint256 amount
    ) external override onlyWhitelist notPaused {
        require(amount > 0, "Bridge: amount must be greater than 0");
        require(getDepositStatus(from), "Bridge: 0 balance");
        delete userDeposit[from];
        emit Burn(from, amount);
    }

    /// @inheritdoc IBridgeBase
    function bridgeBalance() public view override returns (uint256) {
        uint balance = _token.balanceOf(address(this));
        return balance;
    }

    /// @inheritdoc IBridgeBase
    function getDepositStatus(
        address from
    ) public view override returns (bool hasPendingDeposit) {
        return userDeposit[from].amount > 0;
    }

    /// @inheritdoc IBridgeBase
    function getBridgeInfo()
        external
        view
        override
        returns (
            string memory crossBridgeAddress,
            uint256 minDepositAmount,
            uint256 bridgeFees,
            bool bridgeClosed
        )
    {
        crossBridgeAddress = _crossBridgeAddress;
        minDepositAmount = _minDepositAmount;
        bridgeFees = _bridgeFee;
        bridgeClosed = _paused;
    }

    /// @inheritdoc IBridgeBase
    function cancelPendingDeposit() external override {
        require(getDepositStatus(msg.sender), "Bridge: no pending deposit");
        require(
            userDeposit[msg.sender].timestamp < block.timestamp,
            "Bridge: deposit not yet available for withdrawal"
        );
        uint256 amount = userDeposit[msg.sender].amount;
        delete userDeposit[msg.sender];
        _token.safeTransfer(msg.sender, amount);
    }

    /// @inheritdoc IBridgeBase
    function setMinDepositAmount(
        uint256 amount
    ) external override onlyBridgeAdmin {
        require(amount > 0, "Bridge: amount must be greater than 0");
        _minDepositAmount = amount;
    }

    /// @inheritdoc IBridgeBase
    function setBridgeFees(uint256 fee) external override onlyBridgeAdmin {
        _bridgeFee = fee;
    }

    /// @inheritdoc IBridgeBase
    function emergencyPause(bool paused) external override onlyBridgeAdmin {
        _paused = paused;
    }

    /// @inheritdoc IBridgeBase
    function setCrossBridgeContract(
        string calldata newBridgeAddress
    ) external override onlyBridgeAdmin {
        require(
            isAddressLengthEqualTo(newBridgeAddress, _crossChainAddrLength),
            "Bridge: crossBridge address length invalid"
        );
        _crossBridgeAddress = newBridgeAddress;
    }

    /// @inheritdoc IBridgeBase
    function claimFees() external override onlyBridgeAdmin {
        require(
            _generatedFees > 0,
            "Bridge: generated fees must be greater than 0"
        );
        uint generatedFees = _generatedFees;
        _generatedFees = 0;
        _token.safeTransfer(msg.sender, generatedFees);
    }

    /// @inheritdoc IBridgeBase
    function setBridgeAdmin(
        address newBridgeAdmin
    ) external override onlyBridgeAdmin {
        require(
            newBridgeAdmin != address(0),
            "Bridge: new bridge admin is the zero address"
        );
        bridgeAdmin = newBridgeAdmin;
    }

    /// @inheritdoc IBridgeBase
    function addWhitelistedAddress(
        address whitelist_
    ) external override onlyBridgeAdmin {
        require(
            whitelist_ != address(0),
            "Bridge: new whitelist address is the zero address"
        );
        require(
            isWhitelist[whitelist_].maxBucketCapacity == 0,
            "Bridge: address already whitelisted"
        );
        isWhitelist[whitelist_] = BucketCapacity(
            100000 * (10**18),
            0,
            block.timestamp
        );
    }

    /// @inheritdoc IBridgeBase
    function removeWhitelistedAddress(
        address whitelist_
    ) external override onlyBridgeAdmin {
        require(
            isWhitelist[whitelist_].maxBucketCapacity > 0,
            "Bridge: address not whitelisted"
        );
        delete isWhitelist[whitelist_];
    }

    /// @inheritdoc IBridgeBase
    function setBucketCapacity(
        address whitelist_,
        uint256 capacity_
    ) external override onlyBridgeAdmin {
        require(
            isWhitelist[whitelist_].maxBucketCapacity > 0,
            "Bridge: address not whitelisted"
        );
        isWhitelist[whitelist_].maxBucketCapacity = capacity_;
    }

    /// @notice get bucket capacity for whitelist address
    function getBucket(
        address whitelist_
    ) external view returns (BucketCapacity memory) {
        return isWhitelist[whitelist_];
    }

    /// @inheritdoc IBridgeBase
    function addLiquidity(uint256 amount) external override onlyBridgeAdmin {
        require(amount > 0, "Bridge: amount must be greater than 0");
        uint256 transferrableAmount = calculateBurnFee(amount);
        _token.safeTransferFrom(msg.sender, address(this), amount);
        bridgeLiquidity += transferrableAmount;
    }

    /// @inheritdoc IBridgeBase
    function removeLiquidity(uint256 amount) external override onlyBridgeAdmin {
        require(
            bridgeLiquidity >= amount,
            "Bridge: insufficient bridge liquidity"
        );
        require(
            amount <= bridgeBalance(),
            "Bridge: Insufficient funds in bridge"
        );
        bridgeLiquidity -= amount;
        _token.safeTransfer(msg.sender, amount);
    }

    /// @inheritdoc IBridgeBase
    function getBridgeLiquidityAndFees()
        external
        view
        override
        onlyBridgeAdmin
        returns (uint256, uint256)
    {
        return (bridgeLiquidity, _generatedFees);
    }

    /// @dev Returns the length of an address in bytes
    /// @param _str The address to get the length of
    /// @param _length The length to check against
    /// @return boolean value too confirm the address length
    function isAddressLengthEqualTo(
        string memory _str,
        uint _length
    ) private pure returns (bool) {
        bytes memory strBytes = bytes(_str);
        return strBytes.length == _length;
    }

    /// @notice calculate burn fee percentage and return the amount transferrable
    /// @param amount The initial amount to be transferred
    /// @return uint256 The amount transferrable after fee deduction
    function calculateBurnFee(uint256 amount) private view returns (uint256) {
        uint burnPercentage = _token.burnPercentage();
        uint burnAmount = (amount * burnPercentage) / 100;
        return amount - burnAmount;
    }

    /// @notice calculate and update bucketLevel for whitelisted address
    /// @param amount_ the amount to update and check against
    /// @dev used to monitor whitelisted relayers from exceeding limits
    function _updateBucket(uint amount_) private {
        BucketCapacity storage bucket = isWhitelist[msg.sender];
        uint timePassed = block.timestamp - bucket.lastUpdateTimestamp;
        if (timePassed >= TIME_INTERVAL) {
            bucket.bucketLevel = 0;
            bucket.lastUpdateTimestamp = block.timestamp;
        } else {
            uint newBucketState = bucket.bucketLevel + amount_;
            require(
                newBucketState <= bucket.maxBucketCapacity,
                "Bridge: Bucket Capacity exceeded"
            );
        }
        bucket.bucketLevel += amount_;
    }
}