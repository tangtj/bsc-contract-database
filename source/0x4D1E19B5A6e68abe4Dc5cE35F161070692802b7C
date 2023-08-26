/*
    SPDX-License-Identifier: MIT
    A Bankteller Production
    Elephant Money
    Copyright 2023
*/

/*

    Elephant Money Unlimited Marketplace

    Only at https://elephant.money

*/

// File: @openzeppelin/contracts/security/ReentrancyGuard.sol


// OpenZeppelin Contracts (last updated v4.9.0) (security/ReentrancyGuard.sol)

pragma solidity ^0.8.0;

/**
 * @dev Contract module that helps prevent reentrant calls to a function.
 *
 * Inheriting from `ReentrancyGuard` will make the {nonReentrant} modifier
 * available, which can be applied to functions to make sure there are no nested
 * (reentrant) calls to them.
 *
 * Note that because there is a single `nonReentrant` guard, functions marked as
 * `nonReentrant` may not call one another. This can be worked around by making
 * those functions `private`, and then adding `external` `nonReentrant` entry
 * points to them.
 *
 * TIP: If you would like to learn more about reentrancy and alternative ways
 * to protect against it, check out our blog post
 * https://blog.openzeppelin.com/reentrancy-after-istanbul/[Reentrancy After Istanbul].
 */
abstract contract ReentrancyGuard {
    // Booleans are more expensive than uint256 or any type that takes up a full
    // word because each write operation emits an extra SLOAD to first read the
    // slot's contents, replace the bits taken up by the boolean, and then write
    // back. This is the compiler's defense against contract upgrades and
    // pointer aliasing, and it cannot be disabled.

    // The values being non-zero value makes deployment a bit more expensive,
    // but in exchange the refund on every call to nonReentrant will be lower in
    // amount. Since refunds are capped to a percentage of the total
    // transaction's gas, it is best to keep them low in cases like this one, to
    // increase the likelihood of the full refund coming into effect.
    uint256 private constant _NOT_ENTERED = 1;
    uint256 private constant _ENTERED = 2;

    uint256 private _status;

    constructor() {
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Prevents a contract from calling itself, directly or indirectly.
     * Calling a `nonReentrant` function from another `nonReentrant`
     * function is not supported. It is possible to prevent this from happening
     * by making the `nonReentrant` function external, and making it call a
     * `private` function that does the actual work.
     */
    modifier nonReentrant() {
        _nonReentrantBefore();
        _;
        _nonReentrantAfter();
    }

    function _nonReentrantBefore() private {
        // On the first call to nonReentrant, _status will be _NOT_ENTERED
        require(_status != _ENTERED, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _status = _ENTERED;
    }

    function _nonReentrantAfter() private {
        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _status = _NOT_ENTERED;
    }

    /**
     * @dev Returns true if the reentrancy guard is currently set to "entered", which indicates there is a
     * `nonReentrant` function in the call stack.
     */
    function _reentrancyGuardEntered() internal view returns (bool) {
        return _status == _ENTERED;
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


// OpenZeppelin Contracts (last updated v4.9.0) (access/Ownable.sol)

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
     * `onlyOwner` functions. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby disabling any functionality that is only available to the owner.
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

// File: ElephantMarketPlace.sol


pragma solidity ^0.8.0;








interface IElephantNFT {

    function price() external view returns (uint256);

    function round() external view returns (uint256);

    function roundOf(uint256 tokenId) external view returns (uint256);

    function royaltyInfo(uint256 tokenId, uint256 salePrice) external view returns (address, uint256);

    function safeMint(address to) external;

}

interface IElephantNFTMinter {

    function buyForTreasury() external payable;

}


contract ElephantMarketPlace is Ownable, ERC721Holder, ReentrancyGuard  {

    IERC721 public nft;
    IElephantNFTMinter public minter;

    uint256 constant internal precision = 1e18;

    uint256 public totalRevenue;
    uint256 public txs;

    uint256 private totalUnitsSold;
    
    struct Staker {
        uint256[] tokenIds;
        uint256[] soldTokenIds;
        uint256 income;
    }

    /// @dev Create a deposit dependent staking contract for an nft collection and ERC20 reward token
    constructor(IERC721 _nft) {
        nft = _nft;
    }

    /// @notice mapping of a staker to its wallet
    mapping(address => Staker) private stakers;

    /// @notice Mapping from token ID to owner address
    mapping(uint256 => address) private tokenOwner;

    /// @notice List of all staked tokens
    uint256[] private activeListings;

    /// @notice event emitted when a user has staked a nft
    event Staked(address owner, uint256 tokenId);

    /// @notice event emitted when a user has unstaked a nft
    event Unstaked(address owner, uint256 tokenId);

    /// @notice event emitted when a user has purchases a nft
    event Purchased(address seller, address buyer, uint256 tokenId, uint256 price);

    /// @notice event emitted when a token is not available for purchase
    event TokenUnavailable(uint256 tokenId, address buyer, uint256 refund);

    /// @notice event emited when minter is updated 
    event UpdateMinter(address oldMinter, address newMinter);

    receive() payable external {
        payable(msg.sender).transfer(msg.value);
    }

    /// @dev Minter is used to insta buyback for the treasury
    function updateMinter(address _minter) onlyOwner external {
        require(_minter != address(minter), "duplicate address");

        emit UpdateMinter(address(minter), _minter);

        minter = IElephantNFTMinter(payable(_minter));
    }

    /// @dev Return true if buyer is eligible to buy a token
    function isEligible(uint256 _tokenId, address _user) public view returns (bool) {
        return tokenOwner[_tokenId] != _user && tokenOwner[_tokenId] != address(0);
    }

    /// @dev Retrieve the price 
    function price() public view returns (uint256) {
        IElephantNFT _nft = IElephantNFT(address(nft));
        return _nft.price() * 90 / 100;
    }

    /// @dev Retrieves the balance of tokens staked 
    function totalSupply() public view returns (uint256) {
        return nft.balanceOf(address(this));
    } 

    // @dev Retrieves the balance of tokens sold 
    function totalSales() public view returns (uint256) {
        return totalUnitsSold;
    } 

    /// @dev Retrieve listing at specified index
    function getListing(uint256 _index) external view returns (uint256) {
        require(_index < totalSupply(), "index out of range for listing");
        return activeListings[_index];
    }

    /// @dev Retrieve active listings
    function getListings() external view  returns (uint256[] memory tokenIds) {
        return activeListings;
    }

    /// @dev Retrieve the token balance of any single address.
    function balanceOf(address _user) public view returns (uint256) {
        return stakers[_user].tokenIds.length;
    }

    /// @dev Retrieve the sold token balance of any single address.
    function balanceSoldOf(address _user) public view returns (uint256) {
        return stakers[_user].soldTokenIds.length;
    }

    /// @dev Retrieve the total rewards of any single address.
    function totalEarnedOf(address _user) public view returns (uint256) {
        return stakers[_user].income;
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

    /// @dev Retrieve the sold tokenIds of any single address.
    function tokensSoldOfOwner(
        address _owner
    ) public view returns (uint256[] memory tokenIds) {
        return stakers[_owner].soldTokenIds;
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
        for (uint256 i = 0; i < tokenIds.length; i++) {
            if (tokenOwner[tokenIds[i]] == msg.sender) {
                _unstake(msg.sender, tokenIds[i]);
            }
        }

        txs++;
    }

    function buy(address _user, uint256 _tokenId) nonReentrant external payable {

        //These are the hard requirements 
        require(tokenOwner[_tokenId] != _user, "user must not be the owner");

        IElephantNFT _nft = IElephantNFT(address(nft));
        uint256 _price = price();

        require(msg.value >=  _price, "insufficient funds for purchase");

        //token must be staked to complete the purchase
        if (tokenOwner[_tokenId] != address(0)) {
        
            (address _treasury, uint256 _royalty) = _nft.royaltyInfo(_tokenId, _price);
            uint256 _return = msg.value - _price;
            uint256 _income = _price - _royalty; 
            address _owner = tokenOwner[_tokenId];
            Staker storage staker = stakers[_owner];

            //remove _tokenId from list
            for (uint i=0; i < staker.tokenIds.length; i++) {
                if (staker.tokenIds[i] == _tokenId) {
                    //remove ownership
                    staker.tokenIds[i] = staker.tokenIds[staker.tokenIds.length - 1];
                    staker.tokenIds.pop();
                    
                    delete tokenOwner[_tokenId]; //remove ownership

                    //track sold token and income
                    staker.soldTokenIds.push(_tokenId);
                    staker.income += _income;

                    //track globals
                    totalRevenue += _price;
                    totalUnitsSold++; 
                    txs++;
                    break;
                }
            }

            _removeFromActiveListing(_tokenId);

            //Transfer & Log

            //pay owner & treasury; refund if necessary
            payable(_owner).transfer(_income);
            
            //instabuy for treasury or send to specified treasury
            if (address(minter) != address(0)){
                minter.buyForTreasury{value: _royalty}();
            } else {
                payable(_treasury).transfer(_royalty);
            }

            if (_return > 0) {
                payable(msg.sender).transfer(_return);
            }

            //deliver 
            nft.safeTransferFrom(address(this), _user, _tokenId);

            emit Purchased(_owner, _user, _tokenId, _price);

        } else {

            //return sent funds
            payable(msg.sender).transfer(msg.value);

            emit TokenUnavailable(_tokenId, msg.sender, msg.value);
        }

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
        activeListings.push(_tokenId);
        
        tokenOwner[_tokenId] = _user; //assign ownership within this contract

        nft.safeTransferFrom(_user, address(this), _tokenId);

        emit Staked(_user, _tokenId);
    }

    function _unstake(address _user, uint256 _tokenId) internal {
        require(
            tokenOwner[_tokenId] == _user,
            "user must be the owner of the staked nft"
        );

        Staker storage staker = stakers[_user];

        //remove _tokenId from owner list
        for (uint i=0; i < staker.tokenIds.length; i++) {
            if (staker.tokenIds[i] == _tokenId) {
                staker.tokenIds[i] = staker.tokenIds[staker.tokenIds.length - 1];
                staker.tokenIds.pop();

                delete tokenOwner[_tokenId]; //remove ownership

                break;
            }
        }

        _removeFromActiveListing(_tokenId);
        
        nft.safeTransferFrom(address(this), _user, _tokenId);

        emit Unstaked(_user, _tokenId);

    }

    function _removeFromActiveListing(uint256 _tokenId) private {
        for (uint i=0; i < activeListings.length; i++) {
            if (activeListings[i] == _tokenId) {
                activeListings[i] = activeListings[activeListings.length - 1];
                activeListings.pop();
                break;
            }
        }
    }

}