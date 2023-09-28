// SPDX-License-Identifier: MIT

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

// File: @openzeppelin/contracts/security/Pausable.sol


// OpenZeppelin Contracts (last updated v4.7.0) (security/Pausable.sol)

pragma solidity ^0.8.0;


/**
 * @dev Contract module which allows children to implement an emergency stop
 * mechanism that can be triggered by an authorized account.
 *
 * This module is used through inheritance. It will make available the
 * modifiers `whenNotPaused` and `whenPaused`, which can be applied to
 * the functions of your contract. Note that they will not be pausable by
 * simply including this module, only once the modifiers are put in place.
 */
abstract contract Pausable is Context {
    /**
     * @dev Emitted when the pause is triggered by `account`.
     */
    event Paused(address account);

    /**
     * @dev Emitted when the pause is lifted by `account`.
     */
    event Unpaused(address account);

    bool private _paused;

    /**
     * @dev Initializes the contract in unpaused state.
     */
    constructor() {
        _paused = false;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is not paused.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    modifier whenNotPaused() {
        _requireNotPaused();
        _;
    }

    /**
     * @dev Modifier to make a function callable only when the contract is paused.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    modifier whenPaused() {
        _requirePaused();
        _;
    }

    /**
     * @dev Returns true if the contract is paused, and false otherwise.
     */
    function paused() public view virtual returns (bool) {
        return _paused;
    }

    /**
     * @dev Throws if the contract is paused.
     */
    function _requireNotPaused() internal view virtual {
        require(!paused(), "Pausable: paused");
    }

    /**
     * @dev Throws if the contract is not paused.
     */
    function _requirePaused() internal view virtual {
        require(paused(), "Pausable: not paused");
    }

    /**
     * @dev Triggers stopped state.
     *
     * Requirements:
     *
     * - The contract must not be paused.
     */
    function _pause() internal virtual whenNotPaused {
        _paused = true;
        emit Paused(_msgSender());
    }

    /**
     * @dev Returns to normal state.
     *
     * Requirements:
     *
     * - The contract must be paused.
     */
    function _unpause() internal virtual whenPaused {
        _paused = false;
        emit Unpaused(_msgSender());
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

// File: contracts/LandsMarketplace.sol



pragma solidity ^0.8.19;




contract LandsMarketplace is Ownable, Pausable {

    using IterableMapping for IterableMapping.Map;

    IterableMapping.Map private tokenIdToOrder;
    IERC721 public collection;
    uint8 public commissionRate; // 1 = 0.1%

    event OrderFilled(uint128 indexed tokenId, address seller, address buyer, uint128 indexed price);

    constructor(uint8 _commissionRate, address _collection) {
        commissionRate = _commissionRate;
        collection = IERC721(_collection);
    }

function createOrder(uint128 _tokenId, uint128 _startPrice, uint128 _endPrice, uint32 _duration) external {
    require(collection.isApprovedForAll(msg.sender, address(this)), "Not approved for ERC721 token transfer");
    IterableMapping.Order memory order = tokenIdToOrder.get(_tokenId);
    
    if (order.seller == msg.sender) {
        revert("Order already exist");
    } else {
        require(collection.ownerOf(_tokenId) == msg.sender, "Seller not owner");
        IterableMapping.Order memory newOrder = IterableMapping.Order({
            seller: msg.sender,
            tokenId: _tokenId,
            startPrice: _startPrice,
            endPrice: _endPrice,
            duration: _duration,
            startTime: uint32(block.timestamp)
        });
        tokenIdToOrder.set(_tokenId, newOrder);
    }

}

 function fillOrder(uint128 _tokenId) external payable {
    IterableMapping.Order storage order = tokenIdToOrder.get(_tokenId);
    address seller = order.seller;
    uint128 startPrice = order.startPrice;
    uint128 endPrice = order.endPrice;
    uint128 duration = order.duration;

    require(collection.ownerOf(order.tokenId) == seller, "Seller not owner");
    require(collection.isApprovedForAll(seller, address(this)), "Not approved for ERC721 token transfer");

    uint128 currentPrice;
    if (block.timestamp < (order.startTime + duration)) {
        // Calculate the current price based on the linear price decrease
        uint128 elapsedTime = uint32(block.timestamp) - order.startTime;
        uint128 priceDifference;
        if (endPrice > startPrice) {
            priceDifference = endPrice - startPrice;
            currentPrice = startPrice + (elapsedTime * priceDifference / duration);
        } else {
            priceDifference = startPrice - endPrice;
            currentPrice = startPrice - (elapsedTime * priceDifference / duration);
        }     
    } else {
        // The auction has ended; the price is now the end price
        currentPrice = endPrice;
    }

    require(msg.value >= currentPrice, "Insufficient payment value");

    uint128 commission = (currentPrice * commissionRate) / 1000;
    uint128 sellerProceeds = currentPrice - commission;
    payable(seller).transfer(sellerProceeds);
    collection.transferFrom(seller, msg.sender, order.tokenId);
    tokenIdToOrder.remove(_tokenId);

    emit OrderFilled(_tokenId, seller, msg.sender, currentPrice);
}


    function updatePriceOrder(uint128 _tokenId, uint128 _newPrice) external {
        require(_newPrice > 0, "Price must be greater than 0");
        IterableMapping.Order storage order = tokenIdToOrder.get(_tokenId);
        require(order.seller == msg.sender, "Only the seller can update the price");
        require(block.timestamp > (order.startTime + order.duration), "You cannot change the price of the auction, wait until the end of time");
        order.endPrice = _newPrice;
        tokenIdToOrder.set(_tokenId, order);
    }

    function cancelOrder(uint128 _tokenId) external {
        IterableMapping.Order storage order = tokenIdToOrder.get(_tokenId);
        require(order.seller == msg.sender, "Only the seller can cancel the order");
        // Remove the order
        tokenIdToOrder.remove(_tokenId);
    }

    function cancelAnyOrder(uint128 _tokenId) external onlyOwner {
        tokenIdToOrder.remove(_tokenId);
    }

    function removeAllInvalidOrders() external onlyOwner {
        uint32 totalOrders = tokenIdToOrder.size();
        for (uint32 i = 0; i < totalOrders; i++) {
           uint128 tokenId = tokenIdToOrder.getKeyAtIndex(i);
           IterableMapping.Order storage order = tokenIdToOrder.get(tokenId);
            if (collection.ownerOf(order.tokenId) != order.seller) {
                tokenIdToOrder.remove(tokenId);
            }
        }
    }


    function cancelAllOrders() external onlyOwner {
        uint32 totalOrders = tokenIdToOrder.size();
        for (uint32 i = 0; i < totalOrders; i++) {
             uint128 tokenId = tokenIdToOrder.getKeyAtIndex(i);
             tokenIdToOrder.remove(tokenId);
        }
    }

    function getOrders() external view returns (IterableMapping.Order[] memory) {
        uint32 totalOrders = tokenIdToOrder.size();
        IterableMapping.Order[] memory ordersArray = new IterableMapping.Order[](totalOrders);
        uint32 validOrdersIndex = 0;
        for (uint32 i = 0; i < totalOrders; i++) {
            uint128 tokenId = tokenIdToOrder.getKeyAtIndex(i);
            IterableMapping.Order storage order = tokenIdToOrder.get(tokenId);
            if (collection.ownerOf(order.tokenId) == order.seller) {
                ordersArray[validOrdersIndex] = order;
                validOrdersIndex++;
            }
        }
        assembly {
            mstore(ordersArray, validOrdersIndex)
        }
        return ordersArray;
    }

    function getSellerOrders(address _seller) external view returns (IterableMapping.Order[] memory) {
        uint32 totalOrders = tokenIdToOrder.size();
        IterableMapping.Order[] memory sellerOrders = new IterableMapping.Order[](totalOrders);
        uint32 validOrdersIndex = 0;
        for (uint32 i = 0; i < totalOrders; i++) {
            uint128 tokenId = tokenIdToOrder.getKeyAtIndex(i);
            IterableMapping.Order storage order = tokenIdToOrder.get(tokenId);
            if (order.seller == _seller && collection.ownerOf(order.tokenId) == _seller) {
                sellerOrders[validOrdersIndex] = order;
                validOrdersIndex++;
            }
        }
        assembly {
            mstore(sellerOrders, validOrdersIndex)
        }

        return sellerOrders;
    }

    function getOrderByTokenId(uint128 _tokenId) external view returns (IterableMapping.Order memory) {
        IterableMapping.Order storage order = tokenIdToOrder.get(_tokenId);

        if (collection.ownerOf(order.tokenId) == order.seller) {
            return order;
        }
        revert("Order not found for the given plot ID");
    }

    function setCommissionRate(uint8 _newCommissionRate) external onlyOwner {
        commissionRate = _newCommissionRate;
    }

    function withdrawCommission() external onlyOwner {
        uint256 balance = address(this).balance;
        require(balance > 0, "No commission to withdraw");
        payable(owner()).transfer(balance);
}


    function pause() external onlyOwner {
        _pause();
    }

    function unpause() external onlyOwner {
        _unpause();
    }
}

library IterableMapping {

    struct Order {
        address seller;
        uint128 tokenId;
        uint128 startPrice;
        uint128 endPrice;
        uint32 duration;
        uint32 startTime;
    }

    // Iterable mapping from uint32 to Order;
    struct Map {
        uint128[] keys;
        mapping(uint128 => Order) values;
        mapping(uint128 => uint32) indexOf;
        mapping(uint128 => bool) inserted;
    }

    function get(Map storage map, uint128 key) internal view returns (Order storage) {
        return map.values[key];
    }

    function getKeyAtIndex(Map storage map, uint32 index) internal view returns (uint128) {
        return map.keys[index];
    }

    function size(Map storage map) internal view returns (uint32) {
        return uint32(map.keys.length);
    }

    function set(Map storage map, uint128 key, Order memory val) internal {
        if (map.inserted[key]) {
            map.values[key] = val;
        } else {
            map.inserted[key] = true;
            map.values[key] = val;
            map.indexOf[key] = uint32(map.keys.length);
            map.keys.push(key);
        }
    }

    function remove(Map storage map, uint128 key) internal {
        if (!map.inserted[key]) {
            return;
        }
        delete map.inserted[key];
        delete map.values[key];

        uint32 index = map.indexOf[key];
        uint128 lastKey = map.keys[map.keys.length - 1];

        map.indexOf[lastKey] = index;
        delete map.indexOf[key];

        map.keys[index] = lastKey;
        map.keys.pop();
    }
}