/*
    SPDX-License-Identifier: MIT
    A Bankteller Production
    Elephant Money
    Copyright 2023
*/

/**

   ELEPHANT MONEY UNLIMITED STAKING

*/

/***
 *        ________           __                __                       
 *       / ____/ /__  ____  / /_  ____ _____  / /_                      
 *      / __/ / / _ \/ __ \/ __ \/ __ `/ __ \/ __/                      
 *     / /___/ /  __/ /_/ / / / / /_/ / / / / /_                        
 *    /_____/_/\___/ .___/_/ /_/\__,_/_/ /_/\__/                        
 *       /  |/  /_/_/ ____  ___  __  __                                 
 *      / /|_/ / __ \/ __ \/ _ \/ / / /                                 
 *     / /  / / /_/ / / / /  __/ /_/ /                                  
 *    /_/  /_/\____/_/ /_/\___/\__, /                                   
 *       __  __      ___      /____/ __           __                    
 *      / / / /___  / (_)___ ___  (_) /____  ____/ /                    
 *     / / / / __ \/ / / __ `__ \/ / __/ _ \/ __  /                     
 *    / /_/ / / / / / / / / / / / / /_/  __/ /_/ /                      
 *    \____/_/ /_/_/_/_/_/_/_/_/_/\__/\___/\__,_/                       
 *                     / ___// /_____ _/ /__(_)___  ____ _              
 *     ____________    \__ \/ __/ __ `/ //_/ / __ \/ __ `/  ____________
 *    /_____/_____/   ___/ / /_/ /_/ / ,< / / / / / /_/ /  /_____/_____/
 *                   /____/\__/\__,_/_/|_/_/_/ /_/\__, /                
 *                                               /____/                 
 */

// File: @openzeppelin/contracts/token/ERC20/IERC20.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC20/IERC20.sol)

pragma solidity ^0.8.17;

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


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC721/IERC721.sol)

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
    function safeTransferFrom(address from, address to, uint256 tokenId, bytes calldata data) external;

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
    function safeTransferFrom(address from, address to, uint256 tokenId) external;

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
    function transferFrom(address from, address to, uint256 tokenId) external;

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
    function setApprovalForAll(address operator, bool approved) external;

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

// File: @openzeppelin/contracts/token/ERC721/IERC721Receiver.sol


// OpenZeppelin Contracts (last updated v4.6.0) (token/ERC721/IERC721Receiver.sol)

pragma solidity ^0.8.0;

/**
 * @title ERC721 token receiver interface
 * @dev Interface for any contract that wants to support safeTransfers
 * from ERC721 asset contracts.
 */
interface IERC721Receiver {
    /**
     * @dev Whenever an {IERC721} `tokenId` token is transferred to this contract via {IERC721-safeTransferFrom}
     * by `operator` from `from`, this function is called.
     *
     * It must return its Solidity selector to confirm the token transfer.
     * If any other value is returned or the interface is not implemented by the recipient, the transfer will be reverted.
     *
     * The selector can be obtained in Solidity with `IERC721Receiver.onERC721Received.selector`.
     */
    function onERC721Received(
        address operator,
        address from,
        uint256 tokenId,
        bytes calldata data
    ) external returns (bytes4);
}

// File: @openzeppelin/contracts/token/ERC721/utils/ERC721Holder.sol


// OpenZeppelin Contracts (last updated v4.9.0) (token/ERC721/utils/ERC721Holder.sol)

pragma solidity ^0.8.0;


/**
 * @dev Implementation of the {IERC721Receiver} interface.
 *
 * Accepts all token transfers.
 * Make sure the contract is able to use its token with {IERC721-safeTransferFrom}, {IERC721-approve} or {IERC721-setApprovalForAll}.
 */
contract ERC721Holder is IERC721Receiver {
    /**
     * @dev See {IERC721Receiver-onERC721Received}.
     *
     * Always returns `IERC721Receiver.onERC721Received.selector`.
     */
    function onERC721Received(address, address, uint256, bytes memory) public virtual override returns (bytes4) {
        return this.onERC721Received.selector;
    }
}

// File: ElephantNFTStaking.sol


pragma solidity ^0.8.0;





contract ElephantNFTStaking is ERC721Holder {

    IERC20 public rewardsToken;
    IERC721 public nft;

    uint256 constant internal magnitude = 2 ** 64;
    uint256 constant internal precision = 1e18;

    uint256 internal profitPerShare;
    uint256 public totalRewards;
    uint256 public txs;
    
    struct Staker {
        uint256[] tokenIds;
        int256 payouts;
        uint256 rewardsReleased;
    }

    /// @dev Create a deposit dependent staking contract for an nft collection and ERC20 reward token
    constructor(IERC721 _nft, IERC20 _rewardsToken) {
        nft = _nft;
        rewardsToken = _rewardsToken;
    }

    /// @notice mapping of a staker to its wallet
    mapping(address => Staker) private stakers;

    /// @notice Mapping from token ID to owner address

    mapping(uint256 => address) private tokenOwner;

    /// @notice event emitted when a user has staked a nft

    event Staked(address owner, uint256 amount);

    /// @notice event emitted when a user has unstaked a nft
    event Unstaked(address owner, uint256 amount);

    /// @notice event emitted when a user claims reward
    event RewardPaid(address indexed user, uint256 reward);

    // @notic event emitted when funding is sent to contract
    event Fund(address indexed source, uint amount);

    function fund(uint256 _amount) external {

        require(_amount > 0, "must donate positive value");
        require(totalSupply() > 0, "tokens must be staked");
        require(
            rewardsToken.transferFrom(msg.sender, address(this), _amount),
            "fund - failed transfer"
        );        

        //This is the magic right here;
        profitPerShare += (_amount * magnitude) / totalSupply();

        totalRewards += _amount;

        emit Fund(msg.sender, _amount);

    }

    /// @dev Retrieves the balance of tokens staked 
    function totalSupply() public view returns (uint256) {
        return nft.balanceOf(address(this));
    } 

    /// @dev Retrieve the token balance of any single address.
    function balanceOf(address _user) public view returns (uint256) {
        return stakers[_user].tokenIds.length;
    }

    /// @dev Retrieve the total rewards of any single address.
    function totalRewardsOf(address _user) public view returns (uint256) {
        return stakers[_user].rewardsReleased;
    }

    /// @dev Retrieves the owner of any given _tokenID
    function ownerOf(uint256 _tokenId) public view returns (address) {
        return tokenOwner[_tokenId];
    }

    /// @dev Retrieve the tokenIds of any single address.
    function tokensOfOwner(
        address _owner
    ) public view returns (uint256[] memory tokenIds) {
        return stakers[_owner].tokenIds;
    }

    /// @dev Retrieve the rewards balance of any single address.
    function rewardsOf(address _user) public view returns (uint256) {
        return (uint256) ((int256) (profitPerShare * balanceOf(_user)) - stakers[_user].payouts) / magnitude;
    }

     /// @dev The percentage of the
    function percentage(address _user) public view returns (uint256) {
        require(totalSupply() > 0, "no tokens staked");
        return (balanceOf(_user) * precision) / totalSupply(); 
    }

    /// @dev Stakes one or more tokens if owned by the sender
    function stake(uint256[] memory tokenIds) public {
        for (uint256 i = 0; i < tokenIds.length; i++) {
            _stake(msg.sender, tokenIds[i]);
        }

        txs++;
    }

    /// @dev Unstakes one or more tokens if owned by the sender
    function unstake(uint256[] memory tokenIds) public {
        _claimReward(msg.sender);
        for (uint256 i = 0; i < tokenIds.length; i++) {
            if (tokenOwner[tokenIds[i]] == msg.sender) {
                _unstake(msg.sender, tokenIds[i]);
            }
        }

        txs++;
    }

    /// @dev Attempt to claim the available dividends for sender
    function claim() external {
       _claimReward(msg.sender);

       txs++;
    }

    function _stake(address _user, uint256 _tokenId) internal {
        //verifying ownership means the staking contract doesn't own it
        require(
            nft.ownerOf(_tokenId) == _user &&
            (nft.getApproved(_tokenId) == address(this) ||
            nft.isApprovedForAll(_user, address(this))),
            "not owned or approved"
            ); 

        Staker storage staker = stakers[_user];

        staker.tokenIds.push(_tokenId);
        
        tokenOwner[_tokenId] = _user; //assign ownership within this contract

        //adjust payouts to avoid overpayment 
        staker.payouts += int256(profitPerShare);  //PPS * amount, but just 1 NFT

        nft.safeTransferFrom(_user, address(this), _tokenId);

        emit Staked(_user, _tokenId);
    }

    function _claimReward(address _user) internal {

        uint256 _rewardAmount = rewardsOf(_user);

        Staker storage staker = stakers[_user];

        if (_rewardAmount > 0) {
            staker.rewardsReleased += _rewardAmount;
            rewardsToken.transfer(_user, _rewardAmount);

            // update dividend tracker
            staker.payouts += (int256) (_rewardAmount * magnitude);

            emit RewardPaid(_user, _rewardAmount);
        }
    }

    function _unstake(address _user, uint256 _tokenId) internal {
        require(
            tokenOwner[_tokenId] == _user,
            "user must be the owner of the staked nft"
        );

        Staker storage staker = stakers[_user];

        //remove _tokenId from list
        for (uint i=0; i < staker.tokenIds.length; i++) {
            if (staker.tokenIds[i] == _tokenId) {
                staker.tokenIds[i] = staker.tokenIds[staker.tokenIds.length - 1];
                staker.tokenIds.pop();
                break;
            }
        }
        
        delete tokenOwner[_tokenId]; //remove ownership

        //update dividends tracker
        staker.payouts -= int256(profitPerShare); //PPS * amount, but just 1 NFT

        nft.safeTransferFrom(address(this), _user, _tokenId);

        emit Unstaked(_user, _tokenId);

    }

    

}