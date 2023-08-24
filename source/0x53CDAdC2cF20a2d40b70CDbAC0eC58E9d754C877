// File: @openzeppelin/contracts/utils/math/SafeMath.sol


// OpenZeppelin Contracts (last updated v4.9.0) (utils/math/SafeMath.sol)

pragma solidity ^0.8.0;

// CAUTION
// This version of SafeMath should only be used with Solidity 0.8 or later,
// because it relies on the compiler's built in overflow checks.

/**
 * @dev Wrappers over Solidity's arithmetic operations.
 *
 * NOTE: `SafeMath` is generally not needed starting with Solidity 0.8, since the compiler
 * now has built in overflow checking.
 */
library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryAdd(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            uint256 c = a + b;
            if (c < a) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function trySub(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b > a) return (false, 0);
            return (true, a - b);
        }
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, with an overflow flag.
     *
     * _Available since v3.4._
     */
    function tryMul(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
            // benefit is lost if 'b' is also tested.
            // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
            if (a == 0) return (true, 0);
            uint256 c = a * b;
            if (c / a != b) return (false, 0);
            return (true, c);
        }
    }

    /**
     * @dev Returns the division of two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryDiv(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a / b);
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers, with a division by zero flag.
     *
     * _Available since v3.4._
     */
    function tryMod(uint256 a, uint256 b) internal pure returns (bool, uint256) {
        unchecked {
            if (b == 0) return (false, 0);
            return (true, a % b);
        }
    }

    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     *
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a - b;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     *
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        return a * b;
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator.
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return a % b;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {trySub}.
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     *
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b <= a, errorMessage);
            return a - b;
        }
    }

    /**
     * @dev Returns the integer division of two unsigned integers, reverting with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a / b;
        }
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * reverting with custom message when dividing by zero.
     *
     * CAUTION: This function is deprecated because it requires allocating memory for the error
     * message unnecessarily. For custom revert reasons use {tryMod}.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     *
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        unchecked {
            require(b > 0, errorMessage);
            return a % b;
        }
    }
}

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.0;

/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
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
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

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
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

// File: @openzeppelin/contracts/utils/Context.sol


// OpenZeppelin Contracts v4.4.1 (utils/Context.sol)

pragma solidity ^0.8.0;

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via msg.sender and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

// File: @openzeppelin/contracts/access/Ownable.sol


// OpenZeppelin Contracts (last updated v4.7.0) (access/Ownable.sol)

pragma solidity ^0.8.0;


/**
 * @dev Contract module which provides a basic access control mechanism, where
 * there is an account (an owner) that can be granted exclusive access to
 * specific functions.
 *
 * By default, the owner account will be the one that deploys the contract. This
 * can later be changed with {transferOwnership}.
 *
 * This module is used through inheritance. It will make available the modifier
 * `onlyOwner`, which can be applied to your functions to restrict their use to
 * the owner.
 */
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

// File: @openzeppelin/contracts/utils/introspection/IERC165.sol


// OpenZeppelin Contracts v4.4.1 (utils/introspection/IERC165.sol)

pragma solidity ^0.8.0;

/**
 * @dev Interface of the ERC165 standard, as defined in the
 * https://eips.ethereum.org/EIPS/eip-165[EIP].
 *
 * Implementers can declare support of contract interfaces, which can then be
 * queried by others ({ERC165Checker}).
 *
 * For an implementation, see {ERC165}.
 */
interface IERC165 {
    /**
     * @dev Returns true if this contract implements the interface defined by
     * `interfaceId`. See the corresponding
     * https://eips.ethereum.org/EIPS/eip-165#how-interfaces-are-identified[EIP section]
     * to learn more about how these ids are created.
     *
     * This function call must use less than 30 000 gas.
     */
    function supportsInterface(bytes4 interfaceId) external view returns (bool);
}

// File: @openzeppelin/contracts/token/ERC721/IERC721.sol


// OpenZeppelin Contracts (last updated v4.8.0) (token/ERC721/IERC721.sol)

pragma solidity ^0.8.0;


/**
 * @dev Required interface of an ERC721 compliant contract.
 */
interface IERC721 is IERC165 {
    /**
     * @dev Emitted when `tokenId` token is transferred from `from` to `to`.
     */
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);

    /**
     * @dev Emitted when `owner` enables `approved` to manage the `tokenId` token.
     */
    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);

    /**
     * @dev Emitted when `owner` enables or disables (`approved`) `operator` to manage all of its assets.
     */
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);

    /**
     * @dev Returns the number of tokens in ``owner``'s account.
     */
    function balanceOf(address owner) external view returns (uint256 balance);

    /**
     * @dev Returns the owner of the `tokenId` token.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     */
    function ownerOf(uint256 tokenId) external view returns (address owner);

    /**
     * @dev Safely transfers `tokenId` token from `from` to `to`.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must be approved to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId,
        bytes calldata data
    ) external;

    /**
     * @dev Safely transfers `tokenId` token from `from` to `to`, checking first that contract recipients
     * are aware of the ERC721 protocol to prevent tokens from being forever locked.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must exist and be owned by `from`.
     * - If the caller is not `from`, it must have been allowed to move this token by either {approve} or {setApprovalForAll}.
     * - If `to` refers to a smart contract, it must implement {IERC721Receiver-onERC721Received}, which is called upon a safe transfer.
     *
     * Emits a {Transfer} event.
     */
    function safeTransferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    /**
     * @dev Transfers `tokenId` token from `from` to `to`.
     *
     * WARNING: Note that the caller is responsible to confirm that the recipient is capable of receiving ERC721
     * or else they may be permanently lost. Usage of {safeTransferFrom} prevents loss, though the caller must
     * understand this adds an external call which potentially creates a reentrancy vulnerability.
     *
     * Requirements:
     *
     * - `from` cannot be the zero address.
     * - `to` cannot be the zero address.
     * - `tokenId` token must be owned by `from`.
     * - If the caller is not `from`, it must be approved to move this token by either {approve} or {setApprovalForAll}.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 tokenId
    ) external;

    /**
     * @dev Gives permission to `to` to transfer `tokenId` token to another account.
     * The approval is cleared when the token is transferred.
     *
     * Only a single account can be approved at a time, so approving the zero address clears previous approvals.
     *
     * Requirements:
     *
     * - The caller must own the token or be an approved operator.
     * - `tokenId` must exist.
     *
     * Emits an {Approval} event.
     */
    function approve(address to, uint256 tokenId) external;

    /**
     * @dev Approve or remove `operator` as an operator for the caller.
     * Operators can call {transferFrom} or {safeTransferFrom} for any token owned by the caller.
     *
     * Requirements:
     *
     * - The `operator` cannot be the caller.
     *
     * Emits an {ApprovalForAll} event.
     */
    function setApprovalForAll(address operator, bool _approved) external;

    /**
     * @dev Returns the account approved for `tokenId` token.
     *
     * Requirements:
     *
     * - `tokenId` must exist.
     */
    function getApproved(uint256 tokenId) external view returns (address operator);

    /**
     * @dev Returns if the `operator` is allowed to manage all of the assets of `owner`.
     *
     * See {setApprovalForAll}
     */
    function isApprovedForAll(address owner, address operator) external view returns (bool);
}

// File: FinalStakingContract.sol


// ██╗███╗░░██╗███████╗██╗███╗░░██╗██╗████████╗██╗░░░██╗██████╗░██╗░░░░░░█████╗░░█████╗░██╗░░██╗░██████╗░░░░█████╗░██╗
// ██║████╗░██║██╔════╝██║████╗░██║██║╚══██╔══╝╚██╗░██╔╝██╔══██╗██║░░░░░██╔══██╗██╔══██╗██║░██╔╝██╔════╝░░░██╔══██╗██║
// ██║██╔██╗██║█████╗░░██║██╔██╗██║██║░░░██║░░░░╚████╔╝░██████╦╝██║░░░░░██║░░██║██║░░╚═╝█████═╝░╚█████╗░░░░███████║██║
// ██║██║╚████║██╔══╝░░██║██║╚████║██║░░░██║░░░░░╚██╔╝░░██╔══██╗██║░░░░░██║░░██║██║░░██╗██╔═██╗░░╚═══██╗░░░██╔══██║██║
// ██║██║░╚███║██║░░░░░██║██║░╚███║██║░░░██║░░░░░░██║░░░██████╦╝███████╗╚█████╔╝╚█████╔╝██║░╚██╗██████╔╝██╗██║░░██║██║
// ╚═╝╚═╝░░╚══╝╚═╝░░░░░╚═╝╚═╝░░╚══╝╚═╝░░░╚═╝░░░░░░╚═╝░░░╚═════╝░╚══════╝░╚════╝░░╚════╝░╚═╝░░╚═╝╚═════╝░╚═╝╚═╝░░╚═╝╚═╝
//Made by INFINITYBLOCKS.AI


//FUNCTIONS//
// constructor(address _rewardToken, address _nftToken): This function is called when the contract is first deployed. It sets the reward token (ERC20) and the NFT token (ERC721) for the staking process.

// updatePendingRewards(address staker): This is an internal function which calculates and updates the pending rewards for a specific staker. It takes the difference between the current block number and the last checked block number to calculate the passed blocks. Then, it calculates the rewards using the passed blocks, the reward rate per block per NFT and the number of staked NFTs.

// recalculateRewardPerBlockPerNFT(): This internal function recalculates the reward per block per NFT based on the total staked NFTs and the reward rate.

// earned(address account): This public view function calculates and returns the earned rewards of a staker.

// isStakingActive(): This public view function checks if the staking is active by comparing the current block number with the end block.

// setAllowedTokenIds(uint256[] calldata _tokenIds): This is an owner-only function that allows specific NFT tokens to be staked.

// setTimeParameters(): This internal function sets the block parameters related to the staking, including start block, end block and the cool off end block.

// startStaking(uint256 _totalRewardAmount): This owner-only function starts the staking process. It requires the owner to provide the total reward amount and transfers that amount from the owner's account to the contract.

// stakeNFT(uint256 _tokenId): This function is used by users to stake their NFT tokens. It updates the pending rewards of the staker, transfers the NFT from the staker to the contract, and updates the staking statistics.

// unstakeNFT(uint256 _tokenId): This function allows a staker to unstake their NFT tokens. The function updates the staker's pending rewards, removes the unstaked token from the staker's array, and updates the staking statistics.

// claimRewards(): This function allows a staker to claim their earned rewards.

// endStaking(): This owner-only function stops the staking process.

// claimUnclaimedRewards(): This owner-only function allows the owner to claim any unclaimed rewards after the cool-off period.

// emergencyWithdrawTokens(): This owner-only function allows the owner to withdraw any remaining reward tokens from the contract.

// emergencyUnstake(): This owner-only function allows the owner to unstake all staked NFTs in case of an emergency.

// getStakerInfo(address staker): This function returns the information of a staker, including the number of staked NFTs, the first block the staker staked, and the current earned rewards.

// restartStaking(uint256 _additionalRewardAmount): This owner-only function restarts the staking process with additional rewards.

// getTotalRewardAmount(): This function returns the total reward amount left in the contract.

// isInCoolOffPeriod(): This function checks if the contract is in the cool-off period, which is the time after the end block and before the cool-off end block.
//FUNCTIONS//


pragma solidity ^0.8.14;





contract HypcNodeStakingNftWolves is Ownable {
    using SafeMath for uint256;

    IERC20 public rewardToken;
    IERC721 public nftToken;

    struct StakerInfo {
        uint256 lastBlockChecked;
        uint256 nftCount;
        uint256 reward;
        uint256[] stakedTokens;
    }

    mapping(address => StakerInfo) public stakers;
    uint256 public rewardRate;
    uint256 public rewardPerBlockPerNFT;
    uint256 public totalStakedNFTs;
    uint256 public startBlock;
    uint256 public endBlock;
    uint256 public coolOffEndBlock;
    bool public stakingActive;
    mapping(uint256 => bool) public allowedTokenIds;
    bool private isStakingStarted = false;
    address[] public stakerAddresses;
    mapping(address => bool) public isStaker;

    uint256 constant STAKING_DURATION = 28500 * 30;
    uint256 constant COOL_OFF_DURATION = 28500 * 3;

    event Staked(address staker, uint256 tokenId);
    event Unstaked(address staker, uint256 tokenId);
    event Rewards(address staker, uint256 reward);

    constructor(address _rewardToken, address _nftToken) {
        rewardToken = IERC20(_rewardToken);
        nftToken = IERC721(_nftToken);
    }

    function updatePendingRewards(address staker) internal {
        uint256 lastBlockChecked = stakers[staker].lastBlockChecked;
        uint256 blocksToUpdate = block.number > endBlock &&
            lastBlockChecked < endBlock
            ? endBlock
            : block.number;
        uint256 blocksPassed = blocksToUpdate.sub(lastBlockChecked);
        uint256 reward = blocksPassed.mul(rewardPerBlockPerNFT).mul(
            stakers[staker].nftCount
        );
        stakers[staker].reward = stakers[staker].reward.add(reward);
        stakers[staker].lastBlockChecked = blocksToUpdate;
    }

    function recalculateRewardPerBlockPerNFT() internal {
        if (totalStakedNFTs > 0) {
            rewardPerBlockPerNFT = rewardRate.div(totalStakedNFTs);
        } else {
            rewardPerBlockPerNFT = 0;
        }
    }

    function earned(address account) public view returns (uint256) {
        if (block.number <= endBlock) {
            uint256 blocksPassed = block.number.sub(
                stakers[account].lastBlockChecked
            );
            return
                stakers[account].reward.add(
                    blocksPassed.mul(rewardPerBlockPerNFT).mul(
                        stakers[account].nftCount
                    )
                );
        } else {
            uint256 blocksPassed = endBlock.sub(
                stakers[account].lastBlockChecked
            );
            return
                stakers[account].reward.add(
                    blocksPassed.mul(rewardPerBlockPerNFT).mul(
                        stakers[account].nftCount
                    )
                );
        }
    }

    function isStakingActive() public view returns (bool) {
        return block.number < endBlock;
    }

    function setAllowedTokenIds(uint256[] calldata _tokenIds)
        external
        onlyOwner
    {
        for (uint256 i = 0; i < _tokenIds.length; i++) {
            allowedTokenIds[_tokenIds[i]] = true;
        }
    }

    function setTimeParameters() internal {
        startBlock = block.number;
        endBlock = startBlock.add(STAKING_DURATION);
        coolOffEndBlock = endBlock.add(COOL_OFF_DURATION);
    }

    function startStaking(uint256 _totalRewardAmount) external onlyOwner {
        require(!isStakingStarted, "Staking has already been started");
        require(
            rewardToken.transferFrom(
                msg.sender,
                address(this),
                _totalRewardAmount
            ),
            "Token transfer failed"
        );

        rewardRate = _totalRewardAmount.div(STAKING_DURATION);

        setTimeParameters();

        stakingActive = true;
        isStakingStarted = true;
    }

    function stakeNFT(uint256 _tokenId) external {
        require(block.number < endBlock, "Staking period is over");
        require(allowedTokenIds[_tokenId], "Token ID not allowed");
        require(nftToken.ownerOf(_tokenId) == msg.sender, "Not token owner");
        require(
            nftToken.getApproved(_tokenId) == address(this),
            "Token not approved"
        );

        updatePendingRewards(msg.sender);

        nftToken.transferFrom(msg.sender, address(this), _tokenId);
        totalStakedNFTs++;
        stakers[msg.sender].nftCount++;

        if (!isStaker[msg.sender]) {
            stakerAddresses.push(msg.sender);
            isStaker[msg.sender] = true;
        }

        recalculateRewardPerBlockPerNFT();

        stakers[msg.sender].stakedTokens.push(_tokenId);

        emit Staked(msg.sender, _tokenId);
    }

    function unstakeNFT(uint256 _tokenId) external {
    require(
        (!stakingActive) || (block.number > endBlock && block.number < coolOffEndBlock),
        "Unstaking not allowed at this time"
    );
    require(
        nftToken.ownerOf(_tokenId) == address(this),
        "Token not staked"
    );

    updatePendingRewards(msg.sender);

    uint256[] storage stakedTokens = stakers[msg.sender].stakedTokens;
    for (uint256 i = 0; i < stakedTokens.length; i++) {
        if (stakedTokens[i] == _tokenId) {
            stakedTokens[i] = stakedTokens[stakedTokens.length - 1];
            stakedTokens.pop();
            break;
        }
    }
    nftToken.transferFrom(address(this), msg.sender, _tokenId);
    totalStakedNFTs--;
    stakers[msg.sender].nftCount--;

    recalculateRewardPerBlockPerNFT();

    emit Unstaked(msg.sender, _tokenId);
}

    function claimRewards() external {
        updatePendingRewards(msg.sender);
        uint256 reward = stakers[msg.sender].reward;
        require(reward > 0, "No rewards available");
        stakers[msg.sender].reward = 0;
        rewardToken.transfer(msg.sender, reward);
        emit Rewards(msg.sender, reward);
    }

    function endStaking() external onlyOwner {
        stakingActive = false;
        coolOffEndBlock = block.number; // Immediately end the cool-off period
    }

    function claimUnclaimedRewards() external onlyOwner {
        require(block.number >= coolOffEndBlock, "Cool-off period not over");
        uint256 remaining = rewardToken.balanceOf(address(this));
        rewardToken.transfer(msg.sender, remaining);
    }

    function emergencyWithdrawTokens() external onlyOwner {
        uint256 remaining = rewardToken.balanceOf(address(this));
        rewardToken.transfer(msg.sender, remaining);
    }

    function emergencyUnstake() external onlyOwner {
        for (uint256 j = 0; j < stakerAddresses.length; j++) {
            address stakerAddress = stakerAddresses[j];
            if (stakers[stakerAddress].nftCount > 0) {
                uint256[] memory stakedTokens = stakers[stakerAddress]
                    .stakedTokens;
                for (uint256 i = 0; i < stakedTokens.length; i++) {
                    nftToken.transferFrom(
                        address(this),
                        stakerAddress,
                        stakedTokens[i]
                    );
                }
                delete stakers[stakerAddress].stakedTokens;
                stakers[stakerAddress].nftCount = 0;
                isStaker[stakerAddress] = false;
            }
        }
        totalStakedNFTs = 0;
    }

    function getStakerInfo(address staker)
        external
        view
        returns (
            uint256 stakedNFTCount,
            uint256 firstStakedBlock,
            uint256 currentEarnedReward
        )
    {
        StakerInfo memory info = stakers[staker];
        stakedNFTCount = info.nftCount;
        firstStakedBlock = info.lastBlockChecked;
        currentEarnedReward = earned(staker);
    }

function restartStaking(uint256 _additionalRewardAmount) external onlyOwner {
    require(!stakingActive, "Staking is still active");
    require(
        rewardToken.transferFrom(
            msg.sender,
            address(this),
            _additionalRewardAmount
        ),
        "Token transfer failed"
    );

    uint256 _totalRewardAmount = rewardToken.balanceOf(address(this));

    rewardRate = _totalRewardAmount.div(STAKING_DURATION);
    setTimeParameters();

    for (uint256 i = 0; i < stakerAddresses.length; i++) {
        stakers[stakerAddresses[i]].lastBlockChecked = startBlock;
        stakers[stakerAddresses[i]].reward = 0;
    }

    recalculateRewardPerBlockPerNFT();

    stakingActive = true;
}


function getTotalRewardAmount() public view returns (uint256) {
    return rewardToken.balanceOf(address(this));
}

function isInCoolOffPeriod() public view returns (bool) {
    return block.number > endBlock && block.number <= coolOffEndBlock;
}

    function getTokenIdByWallet(address walletAddress) public view returns (uint256[] memory) {
        return stakers[walletAddress].stakedTokens;
    }

}